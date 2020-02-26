---
layout: page
ptitle: QEMU
---
## Quick emulator
```bash
# Create HDD:
qemu-img create -f qcow2 EHC.avi 64G

# Install from iso:
qemu-system-x86_64 -hda EHC.avi -cdrom ~/data/red/Win7.32and64.iso -m 512M

# Start VM:
qemu-system-x86_64 -m 512M -hda EHC.avi

# Suspend VM to file(while running) – IS THIS WORKING?: i guess not 😐
# Go to Ctrl + Alt + 2 and type: savevm /path/to/save.file

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
```
