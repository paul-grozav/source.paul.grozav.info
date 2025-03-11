---
layout: page
ptitle: SELinux (Security-Enhanced Linux)
---
Enable at boot with kernel parameter: `enforcing=1`.

```bash
# Show the context that a process is running in:
ps -efZ | grep sshd
```
