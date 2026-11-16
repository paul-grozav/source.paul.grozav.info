---
layout: page
title: UEFI shell
---

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
