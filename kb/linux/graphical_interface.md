---
layout: page
ptitle: Graphical interface
---
## 1. "X" server

The `xorg` package, only allows you to run `startx` which will allow you to
start a GUI session.
```bash
packages="" &&
packages="${packages} xorg" && # xinit, xauth, xterm
packages="${packages} xserver-xorg-input-mouse" && # mouse driver for Xorg
packages="${packages} xserver-xorg-input-kbd" && # keyboard driver for Xorg
packages="${packages} xserver-xorg-input-libinput" && # main driver
packages="${packages} x11-apps" && # xeyes, xedit, xcalc
packages="${packages} xloadimage" && # xsetbg - set background for x server
packages="${packages} xinput" && # configure x input devices(keyboard,touch)

export DEBIAN_FRONTEND=noninteractive &&
apt-get update &&
apt-get install -y --no-install-recommends ${packages} &&
true
```

Starting the GUI session will require access to `/run/udev`.

## 2. Display/Login manager

WIP
