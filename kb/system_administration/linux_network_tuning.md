---
layout: page
---

```bash
# Cool stuff: https://www.youtube.com/watch?v=ZYCKSN4xf84
NIC(ring_buffer) --HwIRQ--> Kernel(schedule SwIRQ-puts data in buffer) --recv()--> App(Socket)
NIC level: ethtool --statistics
Kernel H/S IRQs: /proc/interrupts & /proc/net/softnet_stat
protocol layers: netstat -s

# Packets lost before OS (in the Network card):
watch -n 1 -t -d "ethtool -S enp49s0f0 | grep -v \"x_queue_\|x_pb_\"" # The adapter firmware level
watch -n 1 -t -d "ethtool --statistics enp49s0f0 | grep -i \"drop\|discard\|buffer\|fifo\""
ethtool --statistics enp97s0f0 | grep -i "drop\|discard\|buffer\|fifo"

# Interrupts should be handled by all CPUs:
egrep "CPU|enp49s0f0" /proc/interrupts | less -NS
# fix with irqbalance

# Increase current hardware settings:
ethtool --show-ring enp97s0f0 # show values
ethtool --set-ring enp97s0f0 rx 4078 tx 4078

# See more errors and drops per interface in and out: cat /proc/net/dev | column -t | less -NS

# Offload in NIC instead of kernel ?

# /proc/net/softnet_stat : one line per CPU and the 1st column is the total number of packets received, 2nd column is backlog overruns and 3rd column is when Soft IRQ ended with traffic left in the NIC.
watch -n 1 -t -d cat /proc/net/softnet_stat
# sysctl -w net.core.netdev_budget=300 # - Can help if 3rd column gets increased

# TCP socket buffers that the app recv()s from:
grep . /proc/sys/net/ipv4/tcp_{r,w}mem
#sysctl -w net.ipv4.tcp_rmem='4096 87380 8388608' 

# For not TCP socket buffers that the app reads from:
grep . /proc/sys/net/core/{r,w}mem_{default,max}
#sysctl -w net.core.rmem_max=$((8*1024*1024))

# We can increase the numbr of sessions we can accept at any given time with this "LISTEN backlog" (netstat -s) and listen(int sockfd, int backlog)
watch -n 1 -t -d "netstat -s | grep err"

# /proc/sys/net/core/somaxconn - Kernel-permitted maximum connections

# ss -npt - look for Recv-Q and Send-Q
# ss -lt - shows the backlog preallocated size in Send-Q column - this is the maximum number of connections that can be in the backlog at once.
# Note that this might be lower than the value given at listen(), because the OS has a limit: sysctl net.core.somaxconn ... that you can adjust
ss -nemapi # See details about every socket on system. For process info, make sure you run as the process owner

Also worth looking at:
(
  cat /proc/sys/net/ipv4//ip_local_port_range # - Ephermal port range define max number of outbound sockets a fost can create from an IP
  cat /proc/sys/net/ipv4/tcp_fin_timeout # - Defines the minimum time a socket will stay in TIME_WAIT state(unusable after being used once)
  # This basically means your system cannot consistently guarantee more than (61000 - 32768) / 60 = 470 sockets per second.
)
net.ipv4.tcp_max_syn_backlog - Number of connections allowed to be in a SYN state. Kernel will drop new SYNs when this limit is hit
cat /proc/net/snmp | less -NS # watch snmp (or nextline)
for prot in $(cat /proc/net/snmp | awk '{print $1}' | rev | cut -c2- | rev | uniq); do grep "^Ip" /proc/net/snmp | awk -v prot="$prot" '{for(i=0; i<NF-1;i++){if(NR==1){keys[i]=$(i+2)}else if(NR==2){values[i]=$(i+2)}}}END{for(i=0; i<NF-1;i++){print prot"_"keys[i]"="values[i]}}'; done
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/performance_tuning_guide/chap-red_hat_enterprise_linux-performance_tuning_guide-networking
https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/6/html/performance_tuning_guide/main-network
txqueuelen - Adapter Transmit Queue Length: https://access.redhat.com/sites/default/files/attachments/20150325_network_performance_tuning.pdf
https://blog.packagecloud.io/eng/2017/02/06/monitoring-tuning-linux-networking-stack-sending-data/
https://blog.packagecloud.io/eng/2016/06/22/monitoring-tuning-linux-networking-stack-receiving-data/

# ====
Look for dropped packets with:
dropwatch -l kas

and/or:
perf record -g -a -e skb:kfree_skb # will create a ./perf.data
perf script # prints ./perf.data
rm -f ./perf.data # remove it

# For connections that could not be added to backlog(listen queue), this counter will be incremented:
# In netstat -s     see x times the listen queue of a socket overflowed or SYNs to LISTEN sockets dropped growing

# Tags: buffer, overflow, packet, TCP, connection, data, kernel, linux, network, interface, queue, socket

# Watch /proc/net/netstat data are more readable with netstat -s
```
