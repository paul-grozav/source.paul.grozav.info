---
layout: page
ptitle: Disk error testing
---

### 1. Check disk for bad blocks using a read-only test
Make sure that your disk is umounted and run the following command:
{% highlight sh %}
badblocks -sv /dev/sda1
{% endhighlight %}
The meaning of
{% highlight sh %}
33.82% done, 23:14 elapsed. (208/0/0 errors)
{% endhighlight %}
is
{% highlight sh %}
<number of read errors>/<number of write errors>/<number of corruption errors>
{% endhighlight %}
For an SSD it takes aroung 9 seconds to test 1GB of disk space.


### 2. Fix bad blocks using fsck:
{% highlight sh %}
fsck.ext4 -c /dev/sda
{% endhighlight %}
I used this on a ext4 filesystem created with `mkfs.ext4 /dev/sda`


### 3. Check S.M.A.R.T. status:
{% highlight sh %}
smartctl -H /dev/sda

smartctl 6.4 2014-10-07 r4002 [x86_64-linux-3.16.0-4-amd64] (local build)
Copyright (C) 2002-14, Bruce Allen, Christian Franke, www.smartmontools.org

=== START OF READ SMART DATA SECTION ===
SMART overall-health self-assessment test result: PASSED
{% endhighlight %}


### 4. HDD Info:
{% highlight sh %}
smartctl -i /dev/sda

smartctl 6.4 2014-10-07 r4002 [x86_64-linux-3.16.0-4-amd64] (local build)
Copyright (C) 2002-14, Bruce Allen, Christian Franke, www.smartmontools.org

=== START OF INFORMATION SECTION ===
Model Family:     Seagate Barracuda 7200.12
Device Model:     ST3500418AS
Serial Number:    6VM8M2GB
LU WWN Device Id: 5 000c50 01ba9e451
Firmware Version: CC38
User Capacity:    500,106,780,160 bytes [500 GB]
Sector Size:      512 bytes logical/physical
Rotation Rate:    7200 rpm
Device is:        In smartctl database [for details use: -P show]
ATA Version is:   ATA8-ACS T13/1699-D revision 4
SATA Version is:  SATA 2.6, 3.0 Gb/s
Local Time is:    Tue Jun 30 10:57:35 2015 EEST

==> WARNING: A firmware update for this drive may be available,
see the following Seagate web pages:
http://knowledge.seagate.com/articles/en_US/FAQ/207931en
http://knowledge.seagate.com/articles/en_US/FAQ/213891en

SMART support is: Available - device has SMART capability.
SMART support is: Enabled
{% endhighlight %}


### 5. More HDD Info:
`smartctl -a /dev/sda`


### 6. Other HDD Info:
`hdparm -I /dev/sda`