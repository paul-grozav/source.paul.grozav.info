---
layout: page
ptitle: Linux network
---
## 1. nmcli
This is the recommended way of setting up your network in CentOS 8.
```bash
# === Devices ===
# Show status:
nmcli device status

# === Connections ===
# Show connections:
nmcli connection show

# Show connection details:
nmcli --pretty connection show pgrozav_190

# Delete connection by name:
nmcli connection delete paul_11

# === VLANs ===
# VLAN Create
nmcli connection add type vlan con-name paul_11 dev enp2s0f0 id 11 ip4 123.122.121.120/24
```
