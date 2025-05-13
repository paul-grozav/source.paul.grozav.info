---
layout: page
ptitle: Unix filesystem
---

# 1\. ls command
When running `ls -l` the output show like `crw-rw-r-- 1 root root 189, 640 Jan 23 09:21 001`. The first character shows the file type.


#### 1.1. File type
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

#### 1.2. Special characters in file names
If you have to handle files with special names, that you can't refer to, you can use the file inode(index node). To see a file's inode you can use `ls -li`. Then you can run `your_cmd "$(find -inum 60301422)"`, to `cd` into it, or remove it, etc. `ls` also supports the `-b, --escape` parameter, which will `print C-style escapes for nongraphic characters`.

Here's a nice example:
{% highlight sh %}
paul@paul:~/test$ echo -ne "\x1\x2\x3\x4\x5\x6\x7\x8\x9\xA\xB\xC\xD\xE\xF\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1A\x1B\x1C\x1D\x1E\x1F\x20\x7F" | xargs -0 mkdir
paul@paul:~/test$ ls -l
total 4
drwxr-xr-x 2 paul paul 4096 Oct 21 14:39 ''$'\001\002\003\004\005\006\a\b\t\n\v\f\r\016\017\020\021\022\023\024\025\026\027\030\031\032\033\034\035\036\037'' '$'\177'
paul@paul:~/test$ cd ./ # Pressing TAB twice for auto-complete will show a different name
^K^L^M^N^O^P^Q^R^S^T^U^V^W^X^Y^Z^[^\^]^^^_ ^?  ^A^B^C^D^E^F^G^H^I                             
paul@paul:~/test$ ls -lib # To find it's inode
total 4
393323 drwxr-xr-x 2 paul paul 4096 Oct 21 14:39 \001\002\003\004\005\006\a\b\t\n\v\f\r\016\017\020\021\022\023\024\025\026\027\030\031\032\033\034\035\036\037\ \177
paul@paul:~/test$ cd "$(find -inum 393323)" # Manipulate it using it's inode
	

�paul@paul:~/test/

�$ pwd
/home/paul/test/


�
	

�paul@paul:~/test/

�$ ls -la
total 8
drwxr-xr-x 2 paul paul 4096 Oct 21 14:45 .
drwxr-xr-x 3 paul paul 4096 Oct 21 14:42 ..
	

�paul@paul:~/test/

�$ cd ..
paul@paul:~/test$ rm -r "$(find -inum 393323)"
paul@paul:~/test$ ls -l
total 0
{% endhighlight %}

# lsattr /mnt/bin/agent
----i---------e------- /mnt/bin/agent
chattr -i # immutability
chattr +i
# getfacl && setfacl
sticky bit
chcon - selinux labels
