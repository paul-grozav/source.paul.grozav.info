---
layout: page
title: GNU debugger
permalink: /kb/linux/gdb
---

1. `#> gdb executable` - start debugging a new instance of your executable.
2. `(gdb) run` - start running the executable again.
3. `(gdb) break my_file.c:7` - create a breakpoint at my_file.c at line 7. It will stop before executing that line.
4. `(gdb) info break` or `info b` - lists all breakpoints.
5. `(gdb) d 2` - deletes breakpoint identified by number 2.
6. `(gdb) next` - Executes one(following) line of code, and prints the one comming after the one it just executed.
7. `(gdb) info locals` - lists all the variables and their current values.
8. `(gdb) info threads` - Lists all the threads, their ID and their status.
9. `(gdb) print a` - print the value of given variable `a`.
10. `(gdb) continue` - Runs from current breakpoint to the next one or untill the end of the program.
11. `(gdb) break program.c:13 thread 28 if bartab > lim` - Sets a conditional breakpoint, only for thread number 28, that will stop the thread at program.c line 13, if and only if `bartab > lim`. Conditional breakpoints slow down the application, or at least this thread, because they have to be evaluated every time it goes through that line/breakpoint to check if it should stop the execution or not.
12. `(gdb) attach 7181` - Attach to process with the given PID.
13. `(gdb) detach` - Detach from where you are currently attached
14. `(gdb) backtrace` or `bt` - Prints the current call stack - list of functions currently running
15. `(gdb) thread 4` or `t 4` - Move the focus to thread 4, so that you can see it's backtrace for example
