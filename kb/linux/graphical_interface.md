---
layout: page
title: Graphical interface
---
## 1. "X" server

The `xorg` package, allows you to start a GUI session.
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

To start the GUI session you can run `startx`, which will require access to
`/run/udev`.

To list the available video modes, supported by your hardware, you can use
`xrandr -d :0`. Then, to change the resolution, to one of the listed sizes you
can run `xrandr -s 1280x960`.

## 2. Display/Login manager

WIP
