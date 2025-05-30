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

---
## Example how to run command every second
```sh
paul@server:/ping_exporter$ cat metrics_push.service 
[Unit]
Description=Push metrics from local exporter to LAN push gateway
[Service]
ExecStart=/usr/bin/logger -i Hello World
# Since this has to start very often, we have to disable the burst limit:
# https://www.freedesktop.org/software/systemd/man/latest/systemd.unit.html#StartLimitIntervalSec=interval
StartLimitBurst=0


paul@server:/ping_exporter$ cat metrics_push.timer
[Unit]
Description=Trigger push metrics from local exporter to LAN push gateway
[Timer]
# start the service this many seconds after each boot.
OnBootSec=1s
# start the service this many seconds after the last time the service was
# started. This is what causes the timer to repeat itself and behave like a cron
# job.
OnUnitActiveSec=1s
# sets the accuracy of the timer. Timers are only as accurate as this field
# sets, and the default is 1 minute (emulates cron). The main reason to not
# demand the best accuracy is to improve power consumption - if SystemD can
# schedule the next run to coincide with other events, it needs to wake the CPU
# less often. The 1ms in the example above is not ideal - I usually set
# accuracy to 1 (1 second) in my sub-minute scheduled jobs, but that would mean
# that if you look at the log showing the "Hello World" messages, you'd see that
# it is often late by 1 second. If you're OK with that, I suggest setting the
# accuracy to 1 second or more.
AccuracySec=1s
[Install]
WantedBy=timers.target
```
Then start it:
```sh
mkdir -p ${HOME}/.config/systemd/user &&
cp ${script_dir}/ping_exporter/metrics_push.service ${HOME}/.config/systemd/user/ &&
cp ${script_dir}/ping_exporter/metrics_push.timer ${HOME}/.config/systemd/user/ &&
systemctl --user daemon-reload &&
systemctl --user enable --now metrics_push.timer
```

---

`systemctl --user` needs to interact with the `/lib/systemd/systemd --user` 
systemd process that runs as your limited user. In order to do that IPC(Inter
Process Communication), it requries a connection to the user bus via DBus. The
DBus socket is in the `${XDG_RUNTIME_DIR}` directory, and it's called `bus`. The
`XDG_RUNTIME_DIR` env var, points to where the user-specific IPC files are,
which is usually `/run/user/$(id -u)`.

So, in order to run `systemctl --user` you will need to have the
`DBUS_SESSION_BUS_ADDRESS` environment variable properly defined, which will not
exist for scripts invoked through CronD.

```sh
export DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/$(id -u)/bus &&
systemctl --user status container__podman__http
```
Otherwise you'll get an error like:
```txt
Failed to connect to bus: No medium found
```

The [D-Bus](https://en.wikipedia.org/wiki/D-Bus) (Desktop-Bus) daemon is usually
started by the system's desktop manager, when you login, when you start your
session. However, if you enable lingering, you are basically asking systemd to
keep the `systemd --user` process running even when the user is not logged in,
meaning that `systemd --user` will be started automatically at boot, and it will
be kept running, even when there is no active session (no local or remote
terminal or GUI), allowing the user to run processes/services, even if no login
occured, or long after the logout occured.

You can check the status of a user's configuration, including if lingering is
enabled, and where the `XDG_RUNTIME_DIR` is configured, by running:
```sh
# To show all
loginctl show-user ${USER}

# To show one of them
loginctl show-user ${USER} --property=Linger
loginctl show-user ${USER} --property=RuntimePath

# To enable lingering
loginctl enable-linger ${USER}
```

XDG stands for [X Desktop Group](https://en.wikipedia.org/wiki/Freedesktop.org).

Also, `systemd --user` is started by PID=1 (systemd) through
`user@1000.service`, which is the User Manager for that UID=1000. And you can
check it with: `systemctl status user@$(id -u).service`. Then `systemd --user`
is managing/starting the `dbus-daemon` instance for and as the limited user. 
