---
layout: page
ptitle: SELinux (Security-Enhanced Linux)
---
Enable at boot with kernel parameter: `enforcing=1`.

```bash
# Show the context that a process is running in:
ps -efZ | grep sshd

# Show security context (a.k.a. security label) of a file
ls -Z file1

# Restore the security context of a file/directory to default(correct one)
restorecon -v -r /

# chcon
# semanage
```
