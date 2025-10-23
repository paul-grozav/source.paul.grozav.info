---
layout: page
title: Control Groups
---
CGroup a.k.a. Control Group is a mechanism for restricting a group of processes.

CentOS example:
```bash
[root@localhost ~]# yum install libcgroup-tools

# Setup CGroup
[root@localhost ~]# cgcreate -g memory:paul_cg1
[root@localhost ~]# systemd-cgtop
[root@localhost ~]# cgset -r memory.limit_in_bytes=$((1024*1024*200)) paul_cg1
[root@localhost ~]# lscgroup | grep paul
memory:/paul_cg1

# Remove CGroup
[root@localhost ~]# cgdelete memory:/paul_cg1
[root@localhost ~]# lscgroup | grep paul
[root@localhost ~]# 

# Set limits to already running process by PID:
[root@localhost ~]# cgclassify -g memory:paul_cg1 1725

# Start new process with limit
[root@localhost ~]# cgexec -g memory:paul_cg1 python -c "' ' * (1024*1024*100)"
[root@localhost ~]# cgexec -g memory:paul_cg1 python -c "' ' * (1024*1024*800)"
Killed
[root@localhost ~]# 

# It is killed by the kernel (dump from kernel logs):
[ 2929.921270] python invoked oom-killer: gfp_mask=0xd0, order=0, oom_score_adj=0                                                     
[ 2929.921309] python cpuset=/ mems_allowed=0
[ 2929.921344] CPU: 0 PID: 1733 Comm: python Not tainted 3.10.0-1127.el7.x86_64 #1                                                    
[ 2929.921353] Hardware name: QEMU Standard PC (i440FX + PIIX, 1996), BIOS 1.12.0-1 04/01/2014                                        
[ 2929.921362] Call Trace:
[ 2929.921434]  [<ffffffffabd7ff85>] dump_stack+0x19/0x1b
[ 2929.921450]  [<ffffffffabd7a8a3>] dump_header+0x90/0x229
[ 2929.921464]  [<ffffffffab83ec7e>] ? mem_cgroup_reclaim+0x4e/0x120                                                                  
[ 2929.921481]  [<ffffffffab7c246e>] oom_kill_process+0x25e/0x3f0
[ 2929.921494]  [<ffffffffab733a41>] ? cpuset_mems_allowed_intersects+0x21/0x30                                                       
[ 2929.921506]  [<ffffffffab840ba6>] mem_cgroup_oom_synchronize+0x546/0x570                                                           
[ 2929.921518]  [<ffffffffab840020>] ? mem_cgroup_charge_common+0xc0/0xc0                                                             
[ 2929.921529]  [<ffffffffab7c2d14>] pagefault_out_of_memory+0x14/0x90                                                                
[ 2929.921538]  [<ffffffffabd78db3>] mm_fault_error+0x6a/0x157
[ 2929.921551]  [<ffffffffabd8d8d1>] __do_page_fault+0x491/0x500
[ 2929.921562]  [<ffffffffabd8d975>] do_page_fault+0x35/0x90
[ 2929.921574]  [<ffffffffabd89ac9>] ? error_swapgs+0xaa/0xc0
[ 2929.921584]  [<ffffffffabd89778>] page_fault+0x28/0x30
[ 2929.921601] Task in /paul_cg1 killed as a result of limit of /paul_cg1                                                             
[ 2929.921619] memory: usage 204800kB, limit 204800kB, failcnt 70
[ 2929.921627] memory+swap: usage 204800kB, limit 9007199254740988kB, failcnt 0                                                       
[ 2929.921633] kmem: usage 0kB, limit 9007199254740988kB, failcnt 0                                                                   
[ 2929.921638] Memory cgroup stats for /paul_cg1: cache:0KB rss:204800KB rss_huge:200704KB mapped_file:0KB swap:0KB inactive_anon:0KB active_anon:204792KB inactive_file:0KB active_file:0KB unevictable:0KB                                                                 
[ 2929.921684] [ pid ]   uid  tgid total_vm      rss nr_ptes swapents oom_score_adj name                                              
[ 2929.921777] [ 1733]     0  1733   235663    51620     116        0             0 python                                            
[ 2929.921787] Memory cgroup out of memory: Kill process 1733 (python) score 980 or sacrifice child                                   
[ 2929.922776] Killed process 1733 (python), UID 0, total-vm:942652kB, anon-rss:204596kB, file-rss:1884kB, shmem-rss:0kB              
```

CGroups are also able to limit the CPU usage.

Other commands:
===
```bash
# == Enable memory control at boot time (disabled by default in debian)
# echo "GRUB_CMDLINE_LINUX=\"cgroup_enable=memory\"" >> /etc/default/grub ; update-grub2

# == control group tops
# systemd-cgtop

# Set CPU shares limit
# cgset -r cpu.shares=40 paul_cg1

# == Set cgrules
#echo "user_name              cpu,memory                     control_group_1" >> /etc/cgrules.conf

# == Start Control Groups RULES ENGine Daemon
# cgrulesengd
```

It seems that systemd sevices can be limited using cgroup by defining a limit in the service file:
```bash
[Service]
MemoryMax=1G # Limit service to 1 gigabyte
```
though, I never tried it.
