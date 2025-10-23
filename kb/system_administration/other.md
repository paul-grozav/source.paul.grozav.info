---
layout: page
title: Other
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

4\. Copy directory recursive and overwrite (aka restore backup)
===
```bash
(
  mkdir -p root/.config/ &&
  echo "A" > root/.config/a &&
  echo "B" > root/.config/b &&
  cp -r root root2 &&
  # Alter root2
  echo "NOT" >> root2/.config/a &&
  echo "NOT" >> root2/.config/b &&
  echo "C" > root2/.config/c &&

  for d in root root2; do
    for f in ${d}/.config/*; do
      echo "===- ${f} -===" &&
      cat ${f}
    done
  done

  echo && echo && echo && echo &&
  # Restore original contents - note that C is still there
  cp --no-target-directory --recursive --verbose root root2 && # works
  # cp root root2 # FAILS - creates root folder inside root2

  for d in root root2; do
    for f in ${d}/.config/*; do
      echo "===- ${f} -===" &&
      cat ${f}
    done
  done

  rm -rf root root2 &&
  exit 0
)
```
will output:
```bash
===- root/.config/a -=== 
A
===- root/.config/b -===
B
===- root2/.config/a -===
A
NOT                                  
===- root2/.config/b -===            
B                       
NOT
===- root2/.config/c -===
C
                         
 
                         
 
'root/.config/a' -> 'root2/.config/a'
'root/.config/b' -> 'root2/.config/b'
===- root/.config/a -===                              
A                            
===- root/.config/b -===        
B                               
===- root2/.config/a -===
A                
===- root2/.config/b -===           
B                                   
===- root2/.config/c -===        
C
```
