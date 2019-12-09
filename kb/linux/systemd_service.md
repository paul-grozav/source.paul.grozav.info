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
ExecStart=bash -c "date >> /home/administrator/date_serv_start"

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
ExecStart=bash -c "sleep 9999 >/dev/null 2>&1"
# ExecStop=...
# ExecReload=...

[Install]
WantedBy=multi-user.target
# ============================================================================ #
{% endhighlight %}
