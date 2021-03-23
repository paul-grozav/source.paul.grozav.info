---
layout: page
ptitle: SystemD service
---

This can be used to run a simple command at startup. See `Restart=no`. Command
`ExecStart` will be ran as `User`.
{% highlight sh %}
root@k8s:~# cat my.service 
# ============================================================================ #
# Author: Tancredi-Paul Grozav <paul@grozav.info>
# my service
# Check status: systemctl status my
# Start: systemctl start my
# Stop: systemctl stop my
# Enable(start at boot): systemctl enable my
# Disable(do not start at boot): systemctl disable my
# Reload service definition: systemctl daemon-reload
# Install service: cp ./my.service /etc/systemd/system/my.service
# ============================================================================ #
[Unit]
Description=My custom service
After=network.target
StartLimitIntervalSec=0

[Service]
Type=simple
Restart=no
User=administrator
ExecStart=/bin/bash -c "date >> /home/administrator/date_serv_start"

[Install]
WantedBy=multi-user.target
# ============================================================================ #
{% endhighlight %}

And here's an example for running a service, that must always be up:
{% highlight sh %}
# ============================================================================ #
# Author: Tancredi-Paul Grozav <paul@grozav.info>
# my service
# Check status: systemctl status my
# Start: systemctl start my
# Stop: systemctl stop my
# Reload service definition: systemctl daemon-reload
# Install service: cp ./my.service /etc/systemd/system/my.service
# Enable(start at boot): systemctl enable my
# Disable(do not start at boot): systemctl disable my
# ============================================================================ #
[Unit]
Description=Start custom exporter
After=network.target
# Give up restarting if the service fails to restart 5 times, in 10 seconds:
StartLimitBurst=5
StartLimitIntervalSec=10

[Service]
Type=simple
Restart=always
# By default, systemd attempts a restart 100ms after the crash. Note !!! that if
# you set RestartSec=3 then it can't fail starting 5 times in 10 seconds.
RestartSec=1
User=administrator
# Group=...
ExecStart=/bin/bash -c " \
  date > /home/administrator/my_start && \
  sleep 9999 >/dev/null 2>&1 \
"
# ExecStop=...
# ExecReload=...

[Install]
WantedBy=multi-user.target
# ============================================================================ #
{% endhighlight %}

Here's an example of how to run a podman container at startup(boot) without the
need to log into any user session:

{% highlight sh %}
# ============================================================================ #
# Author: Tancredi-Paul Grozav <paul@grozav.info>
# my service
# Check status: systemctl status my
# Start: systemctl start my
# Stop: systemctl stop my
# Enable(start at boot): systemctl enable my
# Disable(do not start at boot): systemctl disable my
# Reload service definition: systemctl daemon-reload
# Install service: mkdir -p ~/.config/systemd/user && cp ./prometheus_node_exporter.service ~/.config/systemd/user/prometheus_node_exporter.service
# ============================================================================ #
# Similarly to system units, user units are located in the following
#   directories (ordered by ascending precedence):
#
# - /usr/lib/systemd/user/ - where units provided by installed packages belong.
# - ~/.local/share/systemd/user/ - where units of packages that have been installed in the home directory belong.
# - /etc/systemd/user/ - where system-wide user units are placed by the system administrator.
# - ~/.config/systemd/user/ - where the user puts their own units.
# ============================================================================ #
# See more: https://wiki.archlinux.org/index.php/systemd/User
# After this file update run:
# systemctl --user disable prometheus_node_exporter && systemctl --user stop prometheus_node_exporter && mkdir -p ~/.config/systemd/user && cp /data/container_services/prometheus_node_exporter.service ~/.config/systemd/user && systemctl --user start prometheus_node_exporter && systemctl --user enable prometheus_node_exporter && systemctl --user status prometheus_node_exporter
# ============================================================================ #
[Unit]
Description=Prometheus full node exporter container

[Service]
Restart=always
ExecStart=/usr/bin/podman run -it --rm --name="prometheus_node_exporter" --net="host" --pid="host" -v "/:/host:ro,rslave" quay.io/prometheus/node-exporter --path.rootfs=/host
ExecStop=/usr/bin/podman stop -t0 prometheus_node_exporter

[Install]
WantedBy=default.target
# ============================================================================ #
{% endhighlight %}
