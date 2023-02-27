---
layout: page
ptitle: Processes and /proc filesystem
---

**Work In Progress**

(not only) For system administrators !

Not all the content from this document applies to OpenBSD.

## 1. Processes

Print all processes:
```bash
# ps faux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
...
paul     12648  0.1  0.7 527000 62676 ?        Sl   Jun10   9:37 konsole
paul     12656  0.0  0.0  24672  6640 pts/1    Ss+  Jun10   0:01  \_ /bin/bash
...
```
In the output above you can see the following columns:
1. **USER** - The owner of the process. The process is limited by limits that apply to that user.
2. **PID** - **P**rocess **I**dentifier. A unique natural number.
3. **%CPU** - The CPU usage of that process. 100 means one full core, 650 means 6 cores and a half, and so on.
4. **%MEM** - Percent of total memory(see `free -m`).
5. **VSZ** - Virtual memory size of the process in `KiB` (1024-byte units). All the memory a process can access(including swapped memory).
6. **RSS** - Resident set size, the non-swapped physical memory that a task has used (inkiloBytes). Actual RAM usage (stack & heap).
7. **TTY** - Controlling tty (terminal).
8. **STAT** - Multi-character process state. See section PROCESS STATE CODES for the different values meaning.
```txt
PROCESS STATE CODES
       Here are the different values that the s, stat and state output specifiers (header "STAT" or "S") will display to describe the state of a process:

               D    uninterruptible sleep (usually IO)
               R    running or runnable (on run queue)
               S    interruptible sleep (waiting for an event to complete)
               T    stopped, either by a job control signal or because it is being traced
               W    paging (not valid since the 2.6.xx kernel)
               X    dead (should never be seen)
               Z    defunct ("zombie") process, terminated but not reaped by its parent

       For BSD formats and when the stat keyword is used, additional characters may be displayed:

               <    high-priority (not nice to other users)
               N    low-priority (nice to other users)
               L    has pages locked into memory (for real-time and custom IO)
               s    is a session leader
               l    is multi-threaded (using CLONE_THREAD, like NPTL pthreads do)
               +    is in the foreground process group
```
9. **START** - Time the command started.
10. **TIME** - Accumulated cpu time, user + system.  The display format is usually 'MMM:SS", but can be shifted to the right if the process used more than 999 minutes of cpu time.
11. **COMMAND** - Command with all its arguments as a string. The output in this column may contain spaces. A process marked <defunct> is partly dead, waiting to be fully destroyed by its parent.

Because we used the `f` flag, in the COMMAND column we can see a tree like
structure that tells us about parent-child inter-process relationships. For
example, in the output above, we can see that the `konsole` process started a
`/bin/bash` process. Therefore, the `konsole` process is the parent and the
`/bin/bash` process is the child of `konsole`.

### 1.1. Process threads
```bash
pgrozav:/>ps aux -e -T
USER       PID  SPID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
...
paul     15534 15534  0.1  4.9 2028272 393016 ?      Sl   Jun10   9:43 /home/paul/data/programs/Qt/Qt5.6.0/Tools/QtCreator/bin/qtcreator
paul     15534 15535  0.0  4.9 2028272 393016 ?      Sl   Jun10   0:29 /home/paul/data/programs/Qt/Qt5.6.0/Tools/QtCreator/bin/qtcreator
paul     15534 15554  0.0  4.9 2028272 393016 ?      Sl   Jun10   0:00 /home/paul/data/programs/Qt/Qt5.6.0/Tools/QtCreator/bin/qtcreator
paul     15534 15592  0.0  4.9 2028272 393016 ?      Sl   Jun10   0:00 /home/paul/data/programs/Qt/Qt5.6.0/Tools/QtCreator/bin/qtcreator
paul     15534 15593  0.0  4.9 2028272 393016 ?      Sl   Jun10   0:00 /home/paul/data/programs/Qt/Qt5.6.0/Tools/QtCreator/bin/qtcreator
paul     15534  3552  0.0  4.9 2028272 393016 ?      Sl   Jun10   0:00 /home/paul/data/programs/Qt/Qt5.6.0/Tools/QtCreator/bin/qtcreator
paul     15534  3553  0.0  4.9 2028272 393016 ?      Sl   Jun10   0:00 /home/paul/data/programs/Qt/Qt5.6.0/Tools/QtCreator/bin/qtcreator
paul     15534 21970  0.0  4.9 2028272 393016 ?      Sl   Jun14   0:00 /home/paul/data/programs/Qt/Qt5.6.0/Tools/QtCreator/bin/qtcreator
...

# This also prints thread ID & name
[root@centos7 ~]# ps -eTl
```
In the example above, we ran `ps aux -e -T` and an extra column was added:
`SPID`, which is a light weight process (thread) ID. In the output above we have
the qtcreator application that started 8 threads. The qtcreator is a process
that has the PID of `15534` and each of it’s 8 threads has it’s own thread
identifier(SPID).

#### 2. /proc filesystem

**IF** your operating system supports the proc filesystem, you can see more
details about each process in `/proc/PID` like this:
```bash
pgrozav:/>tree /proc/12656
/proc/12656
├── attr
│   ├── current
│   ├── exec
│   ├── fscreate
│   ├── keycreate
│   ├── prev
│   └── sockcreate
├── autogroup
├── auxv
├── cgroup
├── clear_refs
├── cmdline
├── comm
├── coredump_filter
├── cpuset
├── cwd -> /home/paul
├── environ
├── exe -> /bin/bash
├── fd
│   ├── 0 -> /dev/pts/1
│   ├── 1 -> /dev/pts/1
│   ├── 2 -> /dev/pts/1
│   └── 255 -> /dev/pts/1
├── fdinfo
│   ├── 0
│   ├── 1
│   ├── 2
│   └── 255
├── gid_map
├── io
├── limits
├── loginuid
├── map_files
├── maps
├── mem
├── mountinfo
├── mounts
├── mountstats
├── net
│   ├── anycast6
│   ├── arp
│   ├── bnep
│   ├── connector
│   ├── dev
│   ├── dev_mcast
│   ├── dev_snmp6
│   │   ├── eth0
│   │   ├── lo
│   │   ├── tun3
│   │   ├── virbr0
│   │   ├── virbr0-nic
│   │   ├── virbr1
│   │   ├── virbr1-nic
│   │   ├── vnet0
│   │   └── vnet1
│   ├── fib_trie
│   ├── fib_triestat
│   ├── hci
│   ├── icmp
│   ├── icmp6
│   ├── if_inet6
│   ├── igmp
│   ├── igmp6
│   ├── ip6_flowlabel
│   ├── ip6_mr_cache
│   ├── ip6_mr_vif
│   ├── ip_conntrack
│   ├── ip_conntrack_expect
│   ├── ip_mr_cache
│   ├── ip_mr_vif
│   ├── ip_tables_matches
│   ├── ip_tables_names
│   ├── ip_tables_targets
│   ├── ipv6_route
│   ├── l2cap
│   ├── mcfilter
│   ├── mcfilter6
│   ├── netfilter
│   │   └── nf_log
│   ├── netlink
│   ├── netstat
│   ├── nf_conntrack
│   ├── nf_conntrack_expect
│   ├── nfsfs
│   │   ├── servers
│   │   └── volumes
│   ├── packet
│   ├── protocols
│   ├── psched
│   ├── ptype
│   ├── raw
│   ├── raw6
│   ├── route
│   ├── rpc
│   │   ├── auth.rpcsec.context
│   │   │   ├── channel
│   │   │   └── flush
│   │   ├── auth.rpcsec.init
│   │   │   ├── channel
│   │   │   └── flush
│   │   ├── auth.unix.gid
│   │   │   ├── channel
│   │   │   ├── content
│   │   │   └── flush
│   │   ├── auth.unix.ip
│   │   │   ├── channel
│   │   │   ├── content
│   │   │   └── flush
│   │   ├── nfs
│   │   ├── nfs4.idtoname
│   │   │   ├── channel
│   │   │   ├── content
│   │   │   └── flush
│   │   ├── nfs4.nametoid
│   │   │   ├── channel
│   │   │   ├── content
│   │   │   └── flush
│   │   ├── nfsd
│   │   ├── nfsd.export
│   │   │   ├── channel
│   │   │   ├── content
│   │   │   └── flush
│   │   ├── nfsd.fh
│   │   │   ├── channel
│   │   │   ├── content
│   │   │   └── flush
│   │   └── use-gss-proxy
│   ├── rt6_stats
│   ├── rt_acct
│   ├── rt_cache
│   ├── sco
│   ├── snmp
│   ├── snmp6
│   ├── sockstat
│   ├── sockstat6
│   ├── softnet_stat
│   ├── stat
│   │   ├── arp_cache
│   │   ├── ip_conntrack
│   │   ├── ndisc_cache
│   │   ├── nf_conntrack
│   │   └── rt_cache
│   ├── tcp
│   ├── tcp6
│   ├── udp
│   ├── udp6
│   ├── udplite
│   ├── udplite6
│   ├── unix
│   └── wireless
├── ns
│   ├── ipc -> ipc:[4026531839]
│   ├── mnt -> mnt:[4026531840]
│   ├── net -> net:[4026531956]
│   ├── pid -> pid:[4026531836]
│   ├── user -> user:[4026531837]
│   └── uts -> uts:[4026531838]
├── numa_maps
├── oom_adj
├── oom_score
├── oom_score_adj
├── pagemap
├── personality
├── projid_map
├── root -> /
├── sched
├── sessionid
├── setgroups
├── smaps
├── stack
├── stat
├── statm
├── status
├── syscall
├── task
│   └── 12656
│       ├── attr
│       │   ├── current
│       │   ├── exec
│       │   ├── fscreate
│       │   ├── keycreate
│       │   ├── prev
│       │   └── sockcreate
│       ├── auxv
│       ├── cgroup
│       ├── children
│       ├── clear_refs
│       ├── cmdline
│       ├── comm
│       ├── cpuset
│       ├── cwd -> /home/paul
│       ├── environ
│       ├── exe -> /bin/bash
│       ├── fd
│       │   ├── 0 -> /dev/pts/1
│       │   ├── 1 -> /dev/pts/1
│       │   ├── 2 -> /dev/pts/1
│       │   └── 255 -> /dev/pts/1
│       ├── fdinfo
│       │   ├── 0
│       │   ├── 1
│       │   ├── 2
│       │   └── 255
│       ├── gid_map
│       ├── io
│       ├── limits
│       ├── loginuid
│       ├── maps
│       ├── mem
│       ├── mountinfo
│       ├── mounts
│       ├── ns
│       │   ├── ipc -> ipc:[4026531839]
│       │   ├── mnt -> mnt:[4026531840]
│       │   ├── net -> net:[4026531956]
│       │   ├── pid -> pid:[4026531836]
│       │   ├── user -> user:[4026531837]
│       │   └── uts -> uts:[4026531838]
│       ├── numa_maps
│       ├── oom_adj
│       ├── oom_score
│       ├── oom_score_adj
│       ├── pagemap
│       ├── personality
│       ├── projid_map
│       ├── root -> /
│       ├── sched
│       ├── sessionid
│       ├── setgroups
│       ├── smaps
│       ├── stack
│       ├── stat
│       ├── statm
│       ├── status
│       ├── syscall
│       ├── uid_map
│       └── wchan
├── timers
├── uid_map
└── wchan

29 directories, 211 files
```
TO DO: Explain cwd, exe, task, fd, and other known members

Thanks to:
1. http://man7.org/linux/man-pages/man5/proc.5.html

#### 2.1. Socket file descriptors
{% highlight sh %}
(0)paul@server:/home/paul $ ls -l /proc/23674/fd | grep socket
lrwx——. 1 paul paul 64 May 17 11:37 10 -> socket:[1842046202]
lrwx——. 1 paul paul 64 May 17 11:37 16 -> socket:[1842046212]
lrwx——. 1 paul paul 64 May 17 11:37 18 -> socket:[1842046213]
lrwx——. 1 paul paul 64 May 17 11:37 26 -> socket:[1887900892]
lrwx——. 1 paul paul 64 May 17 11:37 7 -> socket:[1842045162]
lrwx——. 1 paul paul 64 May 17 11:37 8 -> socket:[1897079922]
{% endhighlight %}
You will see that those IDs between square brackets are device ids, and you can
match them with these:
```bash
(0)paul@server:/home/paul $ lsof -i -a -p 23674
COMMAND PID USER FD TYPE DEVICE SIZE/OFF NODE NAME
serverapp 23674 paul 7u IPv4 1842045162 0t0 TCP *:54321 (LISTEN)
serverapp 23674 paul 8u IPv4 1897079922 0t0 TCP s1.paul.grozav.info:54321->some.domain.com:56558 (ESTABLISHED)
serverapp 23674 paul 10u IPv4 1842046202 0t0 TCP s1.paul.grozav.info:54321->some.domain.com:65008 (ESTABLISHED)
serverapp 23674 paul 16u IPv4 1842046212 0t0 TCP s1.paul.grozav.info:54321->some.domain.com:59215 (ESTABLISHED)
serverapp 23674 paul 18u IPv4 1842046213 0t0 TCP s1.paul.grozav.info:54321->some.domain.com:52462 (ESTABLISHED)
serverapp 23674 paul 26u IPv4 1887900892 0t0 TCP s1.paul.grozav.info:54321->some.domain.com:59341 (ESTABLISHED)

# List all open files, with numbers for hostname and ports
[root@centos7 ~]# lsof -P -a -i -n

```
