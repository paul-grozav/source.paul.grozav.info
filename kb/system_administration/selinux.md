---
layout: page
ptitle: SELinux (Security-Enhanced Linux)
---

Boot time kernel parameters:
```txt
selinux - Enable SELinux
selinux=0 - Disable SELinux
enforcing=1 - Set SELinux to Enforcing mode
enforcing=0 - Set SELinux to Permissive mode
```

Runtime commands:

```bash
# Get enforcing mode (Enforcing, Permissive or Disabled)
getenforce

# Set enforcing mode to Enforcing
setenforce 1

# Set enforcing mode to Permissive
setenforce 0

# Completely disable selinux(then reboot to apply):
sed -i 's/^SELINUX=.*$/SELINUX=disabled/g' /etc/selinux/config

# Get more info about the status
sestatus

# Show the context that a process is running in:
ps -efZ | grep sshd

# Show security context (a.k.a. security label) of a file
ls -Z file1

# Security contexts are stored in the form of extended file attributes.

# Restore the security context of a file/directory to default(correct one)
# The default is defined in: /etc/selinux/targeted/contexts/files/file_contexts
restorecon -v -r /

# Set a temporary specific security context to a file
chcon -t httpd_sys_content_t /file

# semanage sets the new security context, in a persistent way.

# For chroot environment (/mnt/root), use:
setfiles /mnt/root /etc/selinux/targeted/contexts/files/file_contexts /mnt/root
```
