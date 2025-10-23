---
layout: page
title: UEFI shell
---

```sh
# List all interfaces:
ipconfig -l
# List one interface:
ipconfig -l eth0
# Request DHCP for eth0
ifconfig -s eth0 dhcp
# Set static IP for eth0
ifconfig -s eth0 static 192.168.0.58 255.255.255.0 192.168.0.1
# Configure DNS for eth0
ifconfig -s eth0 dns 192.168.0.8 192.168.0.9

# Ping to ensure network communication is possible
ping 8.8.8.8

# Reboot the machine
reset

### PXE BOOT ?
>>Start PXE over IPv4.
  Station IP address is 192.168.0.254

  Server IP address is 192.168.0.1
  NBP filename is syslinux.efi
  NBP filesize is 199952 Bytes
 Downloading NBP file...

  NBP file downloaded successfully.
BdsDxe: loading Boot0001 "UEFI PXEv4 (MAC:00B0D063C226)" from PciRoot(0x0)/Pci(0
x2,0x0)/Pci(0x0,0x0)/MAC(00B0D063C226,0x1)/IPv4(0.0.0.0,0x0,DHCP,0.0.0.0,0.0.0.0
,0.0.0.0)
BdsDxe: starting Boot0001 "UEFI PXEv4 (MAC:00B0D063C226)" from PciRoot(0x0)/Pci(
0x2,0x0)/Pci(0x0,0x0)/MAC(00B0D063C226,0x1)/IPv4(0.0.0.0,0x0,DHCP,0.0.0.0,0.0.0.
0,0.0.0.0)
Getting cached packet
My IP is 192.168.0.254

Loading ../bin/linux/aleph/kernel... ok

Failed to load ldlinux.e64BdsDxe: failed to start Boot0001 "UEFI PXEv4 (MAC:00B0
D063C226)" from PciRoot(0x0)/Pci(0x2,0x0)/Pci(0x0,0x0)/MAC(00B0D063C226,0x1)/IPv
4(0.0.0.0,0x0,DHCP,0.0.0.0,0.0.0.0,0.0.0.0): Load Error
```
