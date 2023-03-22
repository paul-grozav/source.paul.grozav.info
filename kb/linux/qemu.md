---
layout: page
ptitle: QEMU
---
## 1. Quick emulator
```bash
# Create HDD:
qemu-img create -f qcow2 EHC.avi 64G

# Install from iso:
qemu-system-x86_64 -hda EHC.avi -cdrom ~/data/red/Win7.32and64.iso -m 512M

# Start VM:
qemu-system-x86_64 -m 512M -hda EHC.avi

# Resume VM from file(at start) – IS THIS WORKING?:
qemu-system-x86_64 -m 512M -hda EHC.avi -loadvm /path/to/save.file

# Install debian using text-based curses mode:
qemu-system-x86_64 -curses -cdrom ./debian-9.9.0-amd64-netinst.iso -m 512M
# Then wait for it to go into 640x480 graphics mode, and press <ESC> (Escape)
# In a few seconds it will drop to text mode. You just have to type:
# install fb=none vga=normal
# And the installation will continue in text mode
# Or you can boot using these flags to run an automated install:
# install fb=none vga=normal auto-install/enable=true preseed/url=https://gitlab.com/tancredi-paul-grozav/snippets/-/raw/master/debian_example.preseed

# Port forward local to vm. In this case local 5556 is forwarded to vm's 22 (ssh)
qemu-system-x86_64 -m 512M -hda ./vm1.img -device e1000,netdev=net0 -netdev user,id=net0,hostfwd=tcp::5556-:22

# Boot from iPXE and display everything to stdio - Note that Ctrl+C will stop the VM
# Use -serial mon:stdio to forward Ctrl+C to the VM instead of stopping the hypervisor.
qemu-system-x86_64 -m 2G -cdrom ./ipxe.iso -boot d -device e1000,netdev=net0,mac=52:55:00:d1:55:01 -netdev user,id=net0,hostfwd=tcp::5556-:22 -serial stdio -display none -machine graphics=off

# Allow VM to use up to 4 CPU cores
qemu-system-x86_64 -smp 4 -hda ./vm.hdd
```

## 2. Going to the QEMU console (mode: 2)
2.1. In Graphical mode(when QEMU runs in it's own window) you can switch to the console, by pressing `Ctrl + Alt + 2`, or by going to the `menu` -> `View` -> `compatmonitor0`

2.2. In `-curses` mode(when QEMU runs in CLI) you can switch to the console, by pressing `ESC , 2`(that is, press escape, then release it and press key 2).

You can do the same key-combination and key 1 for returning to the VGA console(emulated OS output).

If `ESC , 2` does not work, you might want to try `Alt + 2` (that is, press and hold Alt, then press key 2 and then release both) - just make sure that `Alt + 2` isn't defined as a shortcut in your Terminal app(for example for switching to the 2nd terminal tab).

2.3. QEMU console commands
```bash
# Suspend VM to file(while running) – IS THIS WORKING?: i guess not 😐
(qemu console) savevm /path/to/save.file

# Force quit the VM (in case it froze or something)
(qemu console) quit
# or just the command: q

# Send a key combination to the VM (avoid having it catched by your OS)
(qemu console) sendkey ctrl-alt-f1
# Note the "minus" - between the keys
```
See more here:
<a href="https://qemu-project.gitlab.io/qemu/system/monitor.html" target="_blank">https://qemu-project.gitlab.io/qemu/system/monitor.html</a>.
