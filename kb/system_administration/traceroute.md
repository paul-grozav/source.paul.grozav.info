---
layout: page
ptitle: Trace Route
---

[Manual](https://linux.die.net/man/8/traceroute)

Example of usage:
```bash
paul@server:~$ traceroute dns.google.com
traceroute to dns.google.com (8.8.4.4), 30 hops max, 60 byte packets
 1  192.168.0.1 (192.168.0.1)  13.660 ms  13.622 ms  13.609 ms
 2  * * *
 3  10.0.0.1 (10.0.0.1)  20.226 ms  20.213 ms  20.200 ms
 4  10.30.3.33 (10.30.3.33)  26.867 ms  26.855 ms  26.842 ms
 5  10.220.152.72 (10.220.152.72)  25.698 ms  25.687 ms  25.673 ms
 6  10.220.155.46 (10.220.155.46)  26.792 ms 10.220.177.106 (10.220.177.106)  15.519 ms 10.220.155.48 (10.220.155.48)  15.448 ms
 7  72.14.216.212 (72.14.216.212)  17.065 ms 209.85.168.182 (209.85.168.182)  13.148 ms 72.14.216.212 (72.14.216.212)  16.434 ms
 8  74.125.242.241 (74.125.242.241)  13.081 ms 10.23.192.126 (10.23.192.126)  24.398 ms 10.252.246.62 (10.252.246.62)  24.092 ms
 9  dns.google (8.8.4.4)  24.067 ms  23.488 ms  23.466 ms
paul@server:~$ 
```
First line tells us where the packets are going, the IP of the destination host
and other configurations:
```bash
traceroute to dns.google.com (8.8.4.4), 30 hops max, 60 byte packets
```
After that, we have a table with:
```bash
hop_number   host_name_pkg1 (host_ip_pkg1)   rtt_pkg1   host_name_pkg2 (host_ip_pkg2)   rtt_pkg2   host_name_pkg3 (host_ip_pkg3)   rtt_pkg3
```
In other words, by default, traceroute sends 3 identical packets to the
destination, and it will print information (host, ip, rtt) for the route each
packet takes(consecutive packets can take different routes). If the route is the
same, then the host_name and host_ip are omitted for packets 2 and 3.


Each packet has a TTL(Time To Live) attribute, also known as "hop limit".
Routers decrement the TTL value of a packet by one and if the new TTL is >0 then
it will route/forward that packet, and if the TTL =0 then it will discard the
packet, returning the ICMP error message: "ICMP Time Exceeded".


This is the secret that traceroute is relies on. To "exploit" this, traceroute
will create it's first set(of 3) packets with a TTL=1. These packets will reach
the first router and their TTL will be decremented to 0, making the router(s)
drop the packets and send back the ICMP error message/packet. When these errors
get back to the sender(our traceroute program), it can tell who sent the error
packet, thus, knowing the ip of the router(hop).


The traceroute program will also measure the time that passed since it send the
packet, until it got a response to it(the ICMP error message is the response it
is waiting for). That is the time required for the packet to reach the router
(that will drop the packet) + the time required for the error message/packet to
reach back to the traceroute program. This is called the RTT(Round-Trip Time).


The second set of packets that traceroute will send, will have the TTL attribute
equal to: 2. This time, the first router will decrement the TTL to 1 and forward
the packet(s) along the network to their destination. Thus, the packets will
reach their second hop(router), which will decrement their TTL to 0, drop them
and send back that ICMP error packet. Alowing the traceroute to know who is the
second hop and how long it took to reply(RTT).


In the 3rd set of packets that the traceroute will send(with TTL=3), it will
discover the 3rd hop. So, you see, traceroute can gradually discover the next
hops by sending packets with TTL incremented by 1. It will continue to do that
until, either the last hop it found is the destination, or until a maximum
number of hops were attempted to be discovered(up to a maximum TTL value,
defaults to 30).


At each hop(node), traceroute waits for a reply(the ICMP error packet), for a
specified amount of time. If the packet is not received in that amount of time,
traceroute will display an asterisk (*) and moves on to the next hop(TTL).


An ICMP error packet might not arive in time back to traceroute, because of
several reasons. For example, it is most likely that that router is not
configured to reply to ICMP/UDP traffic, or maybe the packets were blocked by
a firewall.


This 3-asterisk result does not mean that the traffic did not pass through that
node. As you can see in the output above, the second hop/node did not reply with
the ICMP error packet, but it did allow the packets to go forward, we know that
because the 3rd node did get the packets and did reply.


If the last lines/hops you see are all asterisks, then it means that it didn't
reach the destination, and you should ask youself what happened after the last
hop/node that replied. Maybe some packets were dropped, if the destination is
up/alive.
