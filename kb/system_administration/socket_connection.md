---
layout: page
ptitle: Socket connection
---

###### 1. Establish a TCP connection
1. The server creates a socket and calls the [listen](http://man7.org/linux/man-pages/man2/listen.2.html) system call.
2. The client calls the [connect](http://man7.org/linux/man-pages/man2/connect.2.html) (blocking) system call.
3. The client OS creates a **pending** connection and sends a `SYN` packet the the destination(server) OS.
4. Server OS receives the SYN packet, sends back to the client a `SYN-ACK` packet and adds a new **pending** connection to the backlog. If the backlog is full(max size given by server at `listen()` time), then client might be refused.(See more about the backlog parameter of [listen](http://man7.org/linux/man-pages/man2/listen.2.html)). What usually happens when the backlog is full, is that the SYN-ACK is sent to the user, the user replies with an ACK(which the server ignores) and the client thinks that the connection is established. However, when the client starts sending data on the new connection. The server does not know of this connection(as it was not accepted from backlog) and will end the communication by sending a `RST`(Reset) packet back to the client.
5. Client OS receives the SYN-ACK packet, marks the connection(created at step 3.) as **established**, and sends back an `ACK` packet to the server. When the connection becomes established the client process will return from the connect() method.
6. Server OS receives the ACK packet, marks the connection(creates at step 4.) as **established**.

##### 2. Receiving data
See:
[YT Linux Net Tuning](https://www.youtube.com/watch?v=ZYCKSN4xf84),
[RH Tuning guide](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/performance_tuning_guide/sect-red_hat_enterprise_linux-performance_tuning_guide-networking-configuration_tools#sect-Red_Hat_Enterprise_Linux-Performance_Tuning_Guide-Configuration_tools-Configuring_the_hardware_buffer),
[RH Packet Reception](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/performance_tuning_guide/s-network-packet-reception),
[Receive Tuning](https://blog.packagecloud.io/eng/2016/06/22/monitoring-tuning-linux-networking-stack-receiving-data/)
###### 2.1. Hardware (NIC) ring-buffer
[Frames](https://en.wikipedia.org/wiki/Frame_(networking)) are read from the
wire and stored in the network card, inside a ring-buffer (a circular data
structure where the new data overwrites the old data).
To see how many frames were lost in the NIC(ring-buffer) you can use
`ethtool -S eth0`. And for every frame read from the wire into the buffer, the
NIC generates a hardware interrupt (HW IRQ). To increase the ring buffer size
you can use: `ethtool --set-ring eth0 rx 4096 tx 4096`. There is a buffer for
each opperation read/write. You can also show the current buffer size and the
maximum size by using: `ethtool --show-ring eth0`. You can also increase the
*device weight* to drain the buffer faster:
[see more](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/performance_tuning_guide/s-network-common-queue-issues#s-network-commonque-nichwbuf).

###### 2.2. Kernel
The HW IRQ makes the kernel schedule a software interrupt (SW IRQ) that will
actually pull the data from that ring-buffer and into the kernel. The SW IRQ can
be seen as a process `ksoftirqd/X`, and it runs continuously to draw the traffic
off the NIC, to avoid data loss caused by the ring-buffer.
Once the data/frame is in the kernel it it goes through the protocol
stack(Ethernet, IP, TCP/UDP, ...) and the data payload is put in the socket
buffer.

###### 2.3. Socket buffer
The socket buffer is the buffer that holds the data until the application calls
[recv()](http://man7.org/linux/man-pages/man2/recv.2.html) to get the data. The
size of this buffer (in bytes) can be set/get from the application by using
[get/set sockopt()](http://man7.org/linux/man-pages/man2/setsockopt.2.html).
If no such option is set the system provides a default value(also in bytes):
`sysctl net.core.rmem_default` and a maximum value:
`sysctl net.core.rmem_max`. You can also see the number of bytes that are in
this buffer, that the application did not consume by using
[ss](http://man7.org/linux/man-pages/man8/ss.8.html) or
[netstat](http://man7.org/linux/man-pages/man8/netstat.8.html). Note that the
buffer size we ask for (using the OS `rmem_default` or `setsockopt()`), is not
the actual size allocated by the kernel. The kernel doubles this value
[*](http://man7.org/linux/man-pages/man7/tcp.7.html#DESCRIPTION), and uses the
extra space for administrative purposes and internal structures. However, the
`getsockopt()` call returns the actual size, that is twice the size you asked
for, but we should stick to half of it, to the size we asked for.

In kernel terms:
1. `sk_rmem_alloc` - Is the number of bytes that are in the buffer, that are not
consumed/received by the application.
2. `sk_rcvbuf` - The total memory (value) returned by the *getsockopt()*
function.

[See more](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/performance_tuning_guide/s-network-commonque-soft),
[How TCP sockets work](https://eklitzke.org/how-tcp-sockets-work)

---

###### 3. Errors explained:
1. **Could not read from socket. Error code is: 104. Error message is:
"Connection reset by peer".** (got this message using boost asio) This happens
when you connect, the server can't store the connection in the backlog(full) and
then you think you're connected(because the server sends the SYN-ACK even if the
backlog was full) and you start writing to the socket. When the server receives
the data on an unknown connection, it will "close" the connection by sending a
RST packet. And this is the error that you get when you try to read the response
from the socket. These errors will increase the *ListenOverflows* "*times the
listen queue of a socket overflowed*" in `netstat -s` or `/proc/net/netstat`
(`column -t /proc/net/netstat | less -NS`).
See more: [ListenDrop](
https://github.com/netdata/netdata/issues/3234#issuecomment-355613706)

2. **X UDP packet receive errors** - These drops happen in the kernel, either
because the packet was corrupted, or because the receive socket buffer was full.
Receive drops can be seen per socket in `cat /proc/net/udp` (last column)(based
on the inode you can match the socket with the output of `ss -panuem` and see
the exact destionation, file descriptor, pid). To distinguish between corrupted
packets and the receive buffer errors you can look at "*X receive buffer
errors*" or you can see the value in `column -t /proc/net/snmp | less -NS` at
*Udp* -> *RcvbufErrors* (*InErrors*-*RcvbufErrors*=number of corrupted packets
that were dropepd).

3. **Broken pipe** - A process receives a SIGPIPE when it attempts to write to a
pipe (named or not) or socket of type SOCK_STREAM that has no reader left. This
usually happens when you try to **write** to a closed socket/connection.

4. **Connection timed out** - A SYN packet was sent but we got no SYN-ACK in
return. This could be that the SYN packet was dropped somewhere along the way,
or, the SYN-ACK was dropped somewhere, while returning. To find out how far your
SYN packet gets, you can use this
[traceroute](/kb/system_administration/traceroute) command:
`traceroute -n -T -p 25 smtp.google.com`.
