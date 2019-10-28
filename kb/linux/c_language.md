---
layout: page
ptitle: C Language
---
System calls: [See all](http://man7.org/linux/man-pages/man2/syscalls.2.html)
- `getpid()` - Gets PID of current process
- `getppid()` - Gets PID of the parrent of the current process
- `printf("Got pid %d .\n", pid)` - Write message and int to stdout
- `fflush(stdout)` - Flush stdout
- `sleep(1)` - Sleep for 1 second
- `execvp("/bin/bash", args)` - execute process with arguments
- `fork()` - create a new process, cloned from the current one

System calls refer to calling functions implemented in the kernel. [Glibc](https://www.gnu.org/software/libc/) simplifies calling those functions(so that you don't have to work with registers and know each function's number). Shell utilities like `chmod`, are merely some [executables](http://git.savannah.gnu.org/cgit/coreutils.git/tree/src/chmod.c) that make the system call.

See also:
 - https://www.slashroot.in/what-is-system-call-in-unix-and-linux