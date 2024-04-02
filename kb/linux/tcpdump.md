---
layout: page
ptitle: TCP dump
---

**Work In Progress**

## 1. Description

The `tcpdump` tool allows you to sniff network packets being sent across 
interfaces, even if they don't use the TCP protocol.

You can see the details of each packet with the `wireshark` tool, by opening a
**p**ackage **cap**ture file (`.pcap`) in wireshark, by piping tcpdump's output
directly into wireshark, or by capturing the packets live, with wireshark,
directly.

Here is the manual page of tcpdump:
https://www.tcpdump.org/manpages/tcpdump.1.html .

Example commands:
```sh
# This will ssh into a remote server, start there tcpdump as root(using sudo),
# then it will output everything to stdout, which is piped into a local
# wireshark instance, for viewing it in a nice GUI.
echo password | SSHPASS=password sshpass -e ssh -o StrictHostKeyChecking=no \
  user@server sudo -S tcpdump -i any -U -s0 -w - 'not port 22' |
  sudo wireshark -k -i -

# Filter by host (both source or destination) and port
$ tcpdump host 192.168.0.2 and port 3306

# Capture and save to a file
$ tcpdump -w $(pwd)/my.pcap -U host 192.168.0.2 and port 3306
```
