---
layout: page
title: QEMU
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
qemu-system-x86_64 -display curses -cdrom ./debian-12.6.0-amd64-netinst.iso -m 512M -machine graphics=off
# Then wait for it to go into 640x480 graphics mode, and press <ESC> (Escape)
# In a few seconds it will drop to text mode. You just have to type:
# install debug vga=off fb=none nomodeset
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

# UEFI boot
qemu-system-x86_64 -drive if=pflash,format=raw,unit=0,file=/usr/share/qemu/OVMF.fd,readonly=on -serial stdio -display none -machine graphics=off -cdrom ~/ipxe.iso
# UEFI with persistent vars (press F2 to enter UEFI setup):
cp /usr/share/OVMF/OVMF_VARS_4M.fd ${HOME}/OVMF_VARS_4M.fd
qemu-system-x86_64 -drive if=pflash,format=raw,unit=0,file=/usr/share/OVMF/OVMF_CODE_4M.fd,readonly=on -drive if=pflash,format=raw,unit=1,file=${HOME}$/OVMF_VARS_4M.fd -boot menu=on
# UEFI with secure boot(note .ms extension from Microsoft's keys that are
# included, and -M q35 which provides a newer machine type that UEFI supports):
qemu-system-x86_64 -M q35 -drive if=pflash,format=raw,unit=0,file=/usr/share/OVMF/OVMF_CODE_4M.ms.fd,readonly=on -drive if=pflash,format=raw,unit=1,file=${HOME}/OVMF_VARS_4M.ms.fd -boot menu=on -serial stdio -display none -machine graphics=off

# UEFI Secure network boot
qemu-system-x86_64 -device virtio-net-pci,netdev=net0 -netdev user,id=net0,net=192.168.88.0/24,tftp=/root/redhat,bootfile=/shimx64.efi -drive if=pflash
,format=raw,unit=0,file=/usr/share/OVMF/OVMF_CODE_4M.ms.fd,readonly=on -drive if=pflash,format=raw,unit=1,file=${HOME}/OVMF_VARS_4M.ms.fd -m 2G -cpu Broadwell -boot n -M q35 -serial stdio -display none -machine graphics=off
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
