---
layout: page
ptitle: tcpdump and wireshark
---

**Work In Progress**

## 1. tcpdump

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
# wireshark instance, for viewing it in a nice GUI. Or just authenticate with a
# key as root directly.
echo password | SSHPASS=password sshpass -e ssh -o StrictHostKeyChecking=no \
  user@server sudo -S tcpdump -i any -U -s0 -w - 'not port 22' |
  sudo wireshark -k -i -

# Filter by host IP (both source or destination) and port
tcpdump host 192.168.0.2 and port 3306

# Filter by host MAC address (both source and destination)
tcpdump ether host e8:2a:ea:44:55:66

# Capture and save to a file
tcpdump -w $(pwd)/my.pcap -U host 192.168.0.2 and port 3306

# Capture just 1 packet, and print the packet contents, not just the headers
tcpdump -c 1 -vvv -A host 192.168.0.2 and port 3306

# Print packet summary for all packets in file
tcpdump -qns 0 -r $(pwd)/my.pcap
```

## 2. wireshark

Wireshark documentation:

- https://www.wireshark.org/docs/wsug_html_chunked/ChWorkBuildDisplayFilterSection.html

```sh
# Open file with wireshark
wireshark $(pwd)/my.pcap
```

Filters in wireshark:
```txt
# Packets with SYN flag set to ON, but ACK flag set to OFF
tcp.flags.syn==1 && tcp.flags.ack==0

# Show communication on TCP connection with index 3
tcp.stream eq 3
```
