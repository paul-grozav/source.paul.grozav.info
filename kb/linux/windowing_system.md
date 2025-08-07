---
layout: page
ptitle: Windowing system
---

## 0. Window(-ing) system
In computing, a
[windowing system](https://en.wikipedia.org/wiki/Windowing_system) (or window
system) is a software suite that manages separately different parts of display
screens. It is a GUI (Graphical User Interface) that implements the WIMP
(windows, icons, menus, pointer) paradigm for a user interface.

The primary component of a Windowing system is the display server, but it
encompasses the other components like the window manager and desktop manager,
but also things like concepts/protocols(for example between the display server
and display clients), libraries (`libX11`).

In Unix-like systems, a common windowing system is the
[X Window system](https://en.wikipedia.org/wiki/X_Window_System) (or simply X,
or X11 because it's currently at [version 11](https://en.wikipedia.org/wiki/X_Window_System#Release_history) of the X protocol and of the system), developed
by the [X.Org](https://x.org)
[Foundation](https://en.wikipedia.org/wiki/X.Org_Foundation). The first version
of X was launched in 1984 and originated from a part of
[Project Athena](https://en.wikipedia.org/wiki/Project_Athena) at MIT
(Massachusetts Institute of Technology).

The X window system started as changes brought to the
[W](https://en.wikipedia.org/wiki/W_Window_System), which was the window system
of the [V operating system](https://en.wikipedia.org/wiki/V_(operating_system)),
which is unrelated to the
[UNIX System V](https://en.wikipedia.org/wiki/UNIX_System_V).

Some X.Org developers have created
[Wayland](https://en.wikipedia.org/wiki/Wayland_(protocol)), as a prospective
replacement for the X Window system. This is a more modern system taking
advantage of GPU and hardware features that didn't exist when X was created in
1980s.

Other examples of window systems include:
- [Mir](https://en.wikipedia.org/wiki/Windowing_system#Mir) - by Canonical
- [SurfaceFlinger](
  https://en.wikipedia.org/wiki/Windowing_system#SurfaceFlinger) - by Google for
  Android
- [Quartz Compositor](https://en.wikipedia.org/wiki/Quartz_Compositor) - by
  Apple for macOS.
- [Desktop Window Manager](
  https://en.wikipedia.org/wiki/Desktop_Window_Manager) - by Microsoft for
  Windows

## 1. Display server
The display server is responsible with rendering the graphics (received from the
display client applications) on the visual display and pass the hardware inputs
from the end-user to the display client applications (like keyboard presses and
mouse clicks). The visual display can be a physical display monitor or a virtual
memory buffer (works entirely in RAM, for cases when a physical monitor is not
present).

The X Window system offers offers as a display server, the
[X.Org Server](https://en.wikipedia.org/wiki/X.Org_Server) which displays to a
physical monitor, but also [Xvfb](https://en.wikipedia.org/wiki/Xvfb) which
displays to a memory buffer. These are 2 distinct binaries, implementing the
same interface/protocol with the clients, the
[X11 protocol](https://en.wikipedia.org/wiki/Windowing_system#X11). 

While X.Org Server can be ran without a window manager, having one greatly
increases convenience and ease of use.

## 2. Window manager

## 3. Desktop manager

## N. The applications
```txt
Your App (Python / Qt / GTK)
        ↓
Toolkit (Qt, GTK, SDL, etc.)
        ↓
Window system (X11 OR Wayland)
        ↓
Kernel (DRM/KMS, GPU drivers)
        ↓
Hardware
```