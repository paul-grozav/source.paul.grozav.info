---
layout: page
---

# 1\. ls command
When running `ls -l` the output show like `crw-rw-r-- 1 root root 189, 640 Jan 23 09:21 001`. The first character shows the file type

## 1.1. File type
Thanks goes to https://linuxconfig.org/identifying-file-types-in-linux .

###### 1.1.1. `-` : regular file
The regular file is a most common file type found on the Linux system. It governs all different files such us text files, images, binary files, shared libraries, etc. You can create a regular file with the `touch file_name` command. To remove a regular file you can use the `rm file_name` command.
###### 1.1.2. `d` : directory
Directory is second most common file type found in Linux. Directory can be created with the `mkdir dir_name` command. To remove an empty directory you can use the `rmdir dir_name` command. If the directory is not empty you can remove it (and it's contents) using the `rm -r dir_name` command.
###### 1.1.3. `c` : character device file
Character and block device files allow users and programs to communicate with hardware peripheral devices.
###### 1.1.4. `b` : block device file
Block devices are similar to character devices. They mostly govern hardware as hard drives, memory, etc.
###### 1.1.5. `s` : local socket file
Local domain sockets are used for communication between processes. Generally, they are used by services such as X windows, syslog and others.
###### 1.1.6. `p` : named pipe
Similarly as Local sockets, named pipes allow communication between two local processes. They can be created by the `mknod` command and removed with the `rm` command.
###### 1.1.7. `l` : links
With links an administrator can assign a file or directory multiple identities. Symbolic link can be though of as a pointer to an original file. There are two types of symbolic links **hard links** and **soft links**
There are two types of links: **hard links** and **symbolic links**. Symbolic links work like a pointer, or reference to a file. You can create them using `ln -s destination_file link_file` and you can remove them using `unlink link_file` or `rm setuo.s`. To learn more about hard and symbolic links you can see https://www.linux.com/learn/intro-to-linux/2017/6/understanding-linux-links .
