---
layout: page
ptitle: Hardware stress testing
---

This should be useful when you want to test a hardware system for any flaws.

### 1. CPU
1.1. **Generic**: `stress-ng --timeout 5s --cpu 0` - sequentially working through all the different CPU stress methods. See `--cpu-method`

1.2. **Floating point**: `stress-ng --timeout 5s --matrix 0`. `0` - will create an instance for each CPU core, or you could specify a certain number of instances.

### 2. Memory
2.1. **Virtual memory stress**: `stress-ng --timeout 5s --vm 2 --vm-bytes 2G --mmap 2 --mmap-bytes 2G --page-in` - will allocate 2x2G for vm + 2x2G for mmap

### 3. Disk
3.1. **Generic**: `stress-ng --timeout 5s --hdd 0` - continually writing, reading and removing temporary files.

### 4. Other
4.1. **Overall/Generic**: `stress-ng --timeout 1m --all 0` - will start all stressors, `nproc` instances of each. This tests a lot of aspects but since the stressors will compete agains each other, it will not test each component as much as testing it individually.

4.2. **High interrupt load**: `stress-ng --timeout 5s --timer 32 --timer-freq 1000000` - will force many hundreds of thousands of interrupts per second, for example, 32 instances at 1MHz.

4.3. **Major page faults**: `stress-ng --timeout 5s --fault 0 --perf` and `stress-ng --timeout 5s --userfaultfd 0 --perf`

4.4. **Run random stressors**: `stress-ng --timeout 5s --random 128` - will start 128 random stressors.

### 5. Per class
Available classes(see `--class`): `cpu cpu-cache device io interrupt filesystem memory network os pipe scheduler vm`.
```bash
stress-ng --timeout 5s --class cpu --all 0
stress-ng --timeout 5s --class cpu-cache --all 0
stress-ng --timeout 5s --class device --all 0
stress-ng --timeout 5s --class io --all 0 # unsuccessful
stress-ng --timeout 5s --class interrupt --all 0
stress-ng --timeout 5s --class filesystem --all 0 # successful, though dnotify fails
stress-ng --timeout 5s --class memory --all 0 # Hangs the PC
stress-ng --timeout 5s --class network --all 0 # unsuccessfull
stress-ng --timeout 5s --class os --all 0 # Does not end?
stress-ng --timeout 5s --class pipe --all 0
stress-ng --timeout 5s --class scheduler --all 0 # unsuccessful
stress-ng --timeout 5s --class vm --all 0

```

Thanks to:
1. `man stress-ng`
2. https://wiki.ubuntu.com/Kernel/Reference/stress-ng
3. https://www.cyberciti.biz/faq/stress-test-linux-unix-server-with-stress-ng/

---
Stress script:
```bash
#!/bin/bash
# ============================================================================ #
# Author: Tancredi-Paul Grozav <paul@grozav.info>
# ============================================================================ #
bytes_per_cpu_core="$(($(free -b | grep ^Mem: | awk '{print $2}')/$(nproc)))" &&
stress-ng --timeout 1m \
  --cpu 0 \
  --matrix 0 \
  --hdd 0 \
  --vm 0 --vm-bytes ${bytes_per_cpu_core} \
  --mmap 0 --mmap-bytes ${bytes_per_cpu_core} --page-in \
&&
stress-ng --timeout 1m --all 0 &&
true

if [ "$?" == "0" ]; then
  echo "[  OK  ] All tests passed."
else
  echo "[FAILED] One test failed."
fi
exit 0
# ============================================================================ #
```
