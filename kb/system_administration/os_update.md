---
layout: page
ptitle: OS update
---

#### 1. Debian
---
1.1 Normal update procedure:
```bash
uname -a &&
lsb_release -a &&
apt-get update &&
apt-get upgrade &&
apt-get dist-upgrade &&
apt autoremove &&
lsb_release -a &&
uname -a
```
1.2. OS major version update procedure:
```bash
uname -a &&
lsb_release -a &&
# No longer in repo list - removed
aptitude search '~i(!~ODebian)' &&
apt-get update &&
apt-get upgrade &&
apt-get dist-upgrade &&

# Database sanity and consistency checks for partially installed, missing and obsolete packages
dpkg -C &&
# Check what packages are held back. Packages On Hold will not be upgraded, which may cause inconsistencies after Buster upgrade.
apt-mark showhold &&
# Before you move to the next part, it is recommended to fix all issues produced by both above commands. The following command might be of a further assistance:
# dpkg --audit &&

# Change codename:
# Backup current cfg file
cp /etc/apt/sources.list /etc/apt/sources.list_backup_$(lsb_release -c | awk '{print $2}')
# sed -i 's/stretch/buster/g' /etc/apt/sources.list
sed -i "s/$(lsb_release -c | awk '{print $2}')/buster/g" /etc/apt/sources.list

# Perform normal update
apt-get update &&
# List upgradable, remove, etc:
# apt list --upgradable &&
apt-get upgrade &&
apt-get dist-upgrade &&
apt autoremove &&
lsb_release -a &&
uname -a
```
