---
layout: page
ptitle: SystemD service
---


{% highlight sh %}
root@k8s:~# cat my.service 
# ============================================================================ #
# Author: Tancredi-Paul Grozav <paul@grozav.info>
# my service
# Check status: systemctl status my
# Start: systemctl start my
# Stop: systemctl stop my
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
