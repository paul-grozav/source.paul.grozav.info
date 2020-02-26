---
layout: page
ptitle: QEMU
---
## Quick EMUlator
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
```
