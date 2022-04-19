---
layout: page
ptitle: Process memory
---

Topics to cover:

VSZ: Virtual memory SiZe - malloc-ing will increase the VSZ
RSS: Resident Set Size - will not be increased by malloc-ing, but will increase only when actually writing to that pre-allocated memory, by the ammount written. So for example you can allocate 10 GiB of RAM and only write 2 GiB to it. That will mean VSZ += 10 GiB and RSS += 2 GiB.

RSS is always smaller than (<) VSZ.

Remember that VSZ and RSS values are not very easy to explain. So, for example while a simple hello world C++ program always starts with the same VSZ = 5924 kB, the RSS is approximately equal to 1872 kB, but it varies on each run. Looking at /proc/PID/smap you can see for example that the [stack] is not always constant. See also: https://stackoverflow.com/questions/67904845/rss-stack-memory-allocation-not-consistent .

Here's an example:
```cpp
#include <unistd.h> // getpid()
#include <iostream>
#include <string>
using namespace ::std;

int main()
{
  cout << "PID=" << getpid() << endl;
  string a = "";
  // At this point ps and top should report that this process is using:
  // VSZ: 5924 kB and RSS ~= 1872 kB (RSS will vary from one run to another)
  getline(cin, a);



  size_t no_bytes = 50*1024*1024; // 50 MB
  cout << "Allocating " << no_bytes << " bytes ..." << endl;
  char * p = (char*)malloc(no_bytes);
  if( p == 0 )
  {
    cout << "Allocation failed" << endl;
    return 1;
  }
  cout << "Allocation succeeded" << endl;
  // This means that VSZ is now: 57128 kB
  getline(cin, a);



  cout << "Filling memory - " << no_bytes << " bytes ..." << endl;
  for(char * c = (char*)p; c < p+no_bytes/2; c++)
  {
    // cout << "c=" << (int*)c << ". *c set to 0" << endl;
    *c = *(char*)(&c);
  }
  cout << "Filling succeeded" << endl;
  // This means that RSS is now ~ 54468 kB
  // RSS += 52712 or 52628 ... this also varies
  getline(cin, a);


  return 0;
}
```


OOM killer
core dump

ram used vs buff+cache vs free
```

---

Here's a small snippet that you can use to run C/C++ code "as a script", just copy and paste it to run it:
```bash
# ============================================================================ #
(
tmp_bin=$(mktemp) &&
mv ${tmp_bin} ${tmp_bin}.bin &&
tmp_bin="${tmp_bin}.bin" &&

tmp_src=$(mktemp) &&
mv ${tmp_src} ${tmp_src}.cpp &&
tmp_src="${tmp_src}.cpp" &&

(cat - <<'EOF'
// --------------------------- CPP source code begin ------------------------ //
#include <iostream>
using namespace ::std;

int main()
{
  cout << "Hello world" << endl;
  return 0;
}
// --------------------------- CPP source code end -------------------------- //
EOF
) > ${tmp_src} &&

g++ ${tmp_src} -O3 -o ${tmp_bin} &&
rm -f ${tmp_src} &&
${tmp_bin};
rm -f ${tmp_bin}
) # Take the following empty line when you copy-paste

# ============================================================================ #
```
