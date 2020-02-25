---
layout: page
ptitle: Linux network
---
## 1. ip
```bash
# Show interfaces:
ip link

# Add VLAN interface:
# Create interface named eth0.11 of type VLAN based on eth0 with ID=11
ip link add link eth0 name eth0.11 type vlan id 11

# Delete interface by name:
ip link del eth0.11
```

## 2. ifconfig
```bash
# Show interfaces with details:
ifconfig -a

# Set interface IP and bring it up:
ifconfig eth1 192.168.0.3/24 up
```

## 3. nmcli
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

## 4. sysctl
```bash
# Get the current value using:
sysctl net.ipv4.conf.eth0/11.rp_filter
```

#### 4.1. VLAN rp_filter=2
Remember that for CentOS 6+ you need to set the `rp_filter` = `2` on the VLAN interface in order to be able to read from it:
```bash
# For VLAN interface eth0.11 do:
sysctl -w net.ipv4.conf.eth0/11.rp_filter=2
```
