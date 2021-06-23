---
layout: page
ptitle: GNU Debugger
---

1. `#> gdb executable` - start debugging a new instance of your executable.
1. `#> gcore PID` - [gcore](http://man7.org/linux/man-pages/man1/gcore.1.html) generates a core dump without killing the process. It will copy the virtual memory of the process from RAM to the disk. This command will generate a file named `core.PID` in the current working directory, where PID is the numeric process identifier.
1. `#> gdb binary core.file` - Inspect a core file, using a binary that generated the dump.
1. `#> gdb binary PID` - Debug a running process.
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
13. `(gdb) detach` - Detach from where you are currently attached.
14. `(gdb) backtrace` or `bt` - Prints the current call stack - list of functions currently running.
15. `(gdb) thread 4` or `t 4` - Move the focus to thread 4, so that you can see it's backtrace for example.
16. `(gdb) frame 8` or `f 8` - Moves to the scope of frame 8 in the current thread.
17. `(gdb) set print pretty on` - Print objects in a more readable format, indented.
1. `(gdb) info functions getSingleInstance` - Search through all function definitions for a function whose name matches the given regex.
1. `(gdb) disassemble namespace::SingletonClass::getSingleInstance` - You can use the `<+0>` address to obtain the address of the singleton object,

---

# Example GDB non-interactive
```bash
# cat test.sh
# ============================================================================ #
# Author: Tancredi-Paul Grozav <paul@grozav.info>
# ============================================================================ #
(
(cat - <<'EOF'
// --------------------------- CPP source code begin ------------------------ //
#include <iostream>
#include <string>
#include <map>

using namespace ::std;

class S{
  S(){ a=0; b="Paul"; }
  string b;
  int a;
public:
  static S& get() { static S s; return s; }
  void inc(){ a++; }
};

int main()
{
  S::get().inc();
  S::get().inc();
  string a = "ana";
  string b = "has";
  string c = "apples";
  map<char, string> m;
  m['a'] = a;
  m['b'] = b;
  m['c'] = c;
  return 0; // break point
}
// --------------------------- CPP source code end -------------------------- //
EOF
) > ./app.cpp &&



(cat - <<'EOF'
# ============================================================================ #
# Load libstdcxx pretty printers
set detach-on-fork
set auto-load safe-path /
python
url = "https://raw.githubusercontent.com/gcc-mirror/gcc/master/libstdc%2B%2B-v3/python/libstdcxx/v6/printers.py"
import urllib
exec(urllib.urlopen(url).read())
try:
    register_libstdcxx_printers(None)
except:
    pass
end

# Debug the app
break ./app.cpp:28
run
info variables .*S::get()::s.*
info locals
set print pretty on
print m
print a

# Print the singleton object
print ((S)('S::get()::s'))
print ((S)('S::get()::s')).b

detach
quit
# ============================================================================ #
EOF
) > ./my.gdb &&



g++ ./app.cpp -O0 -g3 -o ./app.bin &&
#exit 0
gdb --command ./my.gdb ./app.bin;
rm -f ./app.cpp &&
rm -f ./app.bin &&
rm -f ./my.gdb &&
true
) # Take the following empty line when you copy-paste

# ============================================================================ #
```
Prints:
```txt
...
Breakpoint 1 at 0x400f05: file ./app.cpp, line 28.
Breakpoint 1, main () at ./app.cpp:28
28	  return 0; // break point
All variables matching regular expression ".*S::get()::s.*":

Non-debugging symbols:
0x00000000006040f8  guard variable for S::get()::s
0x0000000000604100  S::get()::s
a = "ana"
b = "has"
c = "apples"
m = std::map with 3 elements = {[97 'a'] = "ana", [98 'b'] = "has", [99 'c'] = "apples"}
$1 = std::map with 3 elements = {
  [97 'a'] = "ana",
  [98 'b'] = "has",
  [99 'c'] = "apples"
}
$2 = "ana"
$3 = {
  b = "Paul", 
  a = 2
}
$4 = "Paul"
```
