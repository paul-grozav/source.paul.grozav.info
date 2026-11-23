---
layout: page
title: OpenStack Ironic
---

## Intro
The Ironic component in the OpenStack suite is able to orchestrate physical machines, giving them images to boot/load and injecting secrets into them securely.

## Basics
Start a MariaDB container which OpenStack Ironic needs for its data:
```sh
$ podman run -d \
  --name ironic-db \
  --net host \
  -v /data/research/openstack_ironic/mariadb:/var/lib/mysql:Z \
  -e MARIADB_ROOT_PASSWORD=rootpass \
  -e MARIADB_DATABASE=ironic \
  -e MARIADB_USER=ironic \
  -e MARIADB_PASSWORD=ironic \
  docker.io/library/mariadb:10.11
```

Then start Ironic:
```sh
podman run \
  -it \
  --replace \
  --name ironic-api \
  --net host \
  -v /data/research/openstack_ironic/tftpboot:/tftpboot:z \
  -v /data/research/openstack_ironic/httpboot:/httpboot:z \
  -v /data/research/openstack_ironic/ironic.conf:/etc/ironic/ironic.conf:ro \
  -e IRONIC_CONFIG_FILE=/etc/ironic/ironic.conf \
  quay.io/metal3-io/ironic:latest
```

with this configuration file:
```ini
[DEFAULT]
my_ip = 127.0.0.1
enabled_hardware_types = ipmi
enabled_management_interfaces = ipmitool
enabled_power_interfaces = ipmitool
enabled_boot_interfaces = pxe,ipxe
transport_url = fake://
auth_strategy = noauth
rpc_transport = json-rpc

[json_rpc]
auth_strategy = noauth

[oslo_messaging_notifications]
transport_url = fake://

# Logging
use_syslog = False
debug = True

[database]
# For a lightweight local/testing setup, SQLite works out of the box.
# Replace with mysql+pymysql://user:pass@host/db for production.
connection = mysql+pymysql://ironic:ironic@127.0.0.1/ironic?charset=utf8mb4
#connection = sqlite:////var/lib/ironic/ironic.sqlite

[api]
# Port where the Ironic API service listens
port = 6385
host = 0.0.0.0
auth_strategy = noauth
api_workers = 1

[conductor]
# Interval (in seconds) for conductor to check node states
api_url = http://127.0.0.1:6385
# Ensure the conductor doesn't require queue workers if running standalone
automated_clean = false

[pxe]
# Local paths mapped inside your Podman container volumes
tftp_root = /tftpboot
tftp_server = 127.0.0.1
http_root = /httpboot
http_url = http://127.0.0.1:8088

# IP address of the host serving TFTP/PXE boot files to booting nodes
images_path = /var/lib/ironic/images/
instance_info_subtl = True

[ipmi]
# Force ipmitool to use IPMI v1.5 (lan) instead of v2.0 (lanplus) globally
default_ipmi_protocol = lan
```

Then inside ironic's container, run: `bash-5.1# ironic-dbsync --config-file /etc/ironic/ironic.conf upgrade` , to initiate the DB.

Then start ironic's components:
- api : `ironic-api --config-file /etc/ironic/ironic.conf &`
- conductor: `ironic-conductor --config-file /etc/ironic/ironic.conf &`

Then you can start using the openstack CLI:
`$ pip install python-openstackclient python-ironicclient --break-system-packages`
Make sure you exported these environment variables: `export OS_AUTH_TYPE=noauth` and `export OS_ENDPOINT=http://127.0.0.1:6385/`.

Then you can list your bare metal nodes using: `baremetal node list` . The list is empty, of course.

But you can create a new node with: `baremetal node create --driver ipmi --name qemu-vm-01` .

## Creating a node with local QEMU and a simulated IPMI
Create this `openipmi_sim.conf` configuration file:
```ini
name "ipmisim1"
user 2 true "admin" "password" admin 10 none md2 md5 straight
startlan 1
addr 127.0.0.1 6230
priv_limit admin
allowed_auths_callback none md2 md5 straight
allowed_auths_user none md2 md5 straight
allowed_auths_operator none md2 md5 straight
allowed_auths_admin none md2 md5 straight
endlan
```

And this command file:
```txt
# Emulation setup file for the IPMI simulator
mc_setbmc 0x20
mc_add 0x20 0 no-device-sdrs 0x23 9 8 0x9f 0x1291 0xf02 persist_sdr
mc_enable 0x20
```

Make sure you create a folder for it's persitence: `mkdir -p ipmi_persist` . Then start the IPMI simulator (can be installed with: `sudo apt install -y openipmi`) using this command: `ipmi_sim -c $(pwd)/openipmi_sim.conf -s $(pwd)/ipmi_persist -f $(pwd)/openipmi_sim.emu` .

Create your node with:
```sh
baremetal node create \
  --driver ipmi \
  --name physical-desktop-01 \
  --driver-info ipmi_address=127.0.0.1 \
  --driver-info ipmi_port=6230 \
  --driver-info ipmi_username=admin \
  --driver-info ipmi_password=password
```
Take notes of the new node's `uuid` then use it to see the machine state: `baremetal node show 8c5fd659-44f5-46c1-89db-3e050b114c60`.
If it was able to connect to IPMI it should turn `provision_state` from `verifying` which is verifying it's state through IPMI to `enroll` which is waiting for it to enroll (boot and contact the ironic system).

You can check ipmi's power state too, using `ipmitool -I lan -H 127.0.0.1 -p 6230 -U admin -P password chassis power status` you should see the message: `Chassis Power is off`.

Ensure you run this in your ironic container, to make it work with ipmi 1.5 (lan) not 2.0 (lanplus).
```sh
bash -c '
mv /usr/bin/ipmitool /usr/bin/ipmitool.real &&
cat << "EOF" > /usr/bin/ipmitool
#!/bin/bash
args=("$@")
new_args=()
skip_next=false
for i in "${!args[@]}"; do
  if [ "$skip_next" = true ]; then
    skip_next=false
    continue
  fi
  if [ "${args[i]}" = "lanplus" ]; then
    new_args+=("lan")
  elif [ "${args[i]}" = "-L" ]; then
    skip_next=true
  else
    new_args+=("${args[i]}")
  fi
done
exec /usr/bin/ipmitool.real "${new_args[@]}"
EOF
chmod +x /usr/bin/ipmitool
'
```
Then run:
```sh
baremetal node set $NODE --driver-info ipmi_protocol=lan --driver-info ipmi_privlevel=admin
baremetal node abort $NODE
baremetal node manage $NODE
baremetal node provide $NODE
```
Now node should be in `provision_state` : `available`

### Try also
```sh
baremetal node set $NODE --boot-interface pxe
baremetal node set $NODE \
  --instance-info kernel=http://alice.home.server.paul.grozav.info:1027/bin/boot/ipxe/bin/os/aleph/latest/aleph.krnl \
  --instance-info ramdisk=http://alice.home.server.paul.grozav.info:1027/bin/boot/ipxe/bin/os/aleph/latest/aleph.ird
baremetal node deploy $NODE
baremetal node maintenance set $NODE
baremetal node maintenance unset $NODE
```

