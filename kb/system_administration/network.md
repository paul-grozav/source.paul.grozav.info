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

# Set interface IP:
ip addr add 192.168.100.1/24 dev eth0.11

# Show configured address for all interfaces:
ip addr show
# or just for one interface by name
ip addr show eth0.11

# Bring interface up/down
ip link set dev eth0.11 up
ip link set dev eth0.11 down
```

## 2. ifconfig
Deprecated in favor of the `ip` command.
```bash
# Show interfaces with details:
ifconfig -a

# Set interface IP and bring it up:
ifconfig eth1 192.168.0.3/24 up
```

## 3. nmcli
This only manages the interfaces managed by NetworkManager.
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

## 5. tcpdump
```bash
# Capture with tcpdump remotely over ssh, and view locally in wireshark
echo root_password | SSHPASS=root_password sshpass -e ssh user@server1 sudo -S tcpdump -i any -U -s0 -w - 'not port 22' | sudo wireshark -k -i -
```

## 6. iftop
```sh
sudo iftop -nN -pP -i eth0 -f "host 192.168.0.7 and port 3306"
```
