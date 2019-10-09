---
layout: page
---

1\. Binary compatibility
===
1.1. GNU Lib C
===
What the system provides:
```bash
# CentOS
me@paul:/$ rpm -qa | grep ^glibc
glibc-2.12-1.149.el6.x86_64
glibc-common-2.12-1.149.el6.x86_64
```
```bash
# Debian
paul@pgrozav:/$ dpkg -l | grep libc6
ii  libc6:amd64                           2.24-11+deb9u3                                 amd64        GNU C Library: Shared libraries
ii  libc6-dbg:amd64                       2.24-11+deb9u3                                 amd64        GNU C Library: detached debugging symbols
ii  libc6-dev:amd64                       2.24-11+deb9u3                                 amd64        GNU C Library: Development Libraries and Header Files
```
What your dynamically linked executable requires:
```bash
me@paul:/$ objdump -T ./executable  | grep GLIBC_ | awk '{print $(NF-1)}' | sort | uniq
GLIBC_2.14
GLIBC_2.2.5
GLIBC_2.3
GLIBC_2.7
```
1.2. Kernel
===
Even for statically linked executables you might reach into issues like:
```bash
me@paul:/$ ./my.executable
FATAL: kernel too old
```

2\. Other commands
===
```bash
# Convert IP to domain
dig -x 192.168.0.6
# Convert domain to IP
host -a paul.grozav.info
# Convert domain to IP using a specific DNS server
host -a paul.grozav.info 8.8.8.8
```

3\. Bash operators
===
```bash
[paul@paul /]$ [ 1 == 1 ] && echo true || echo false ; echo after
true
after
[paul@paul /]$ [ 1 == 2 ] && echo true || echo false ; echo after
false
after
```
