---
layout: page
ptitle: Disk management
---

## 1. fdisk
```bash
# List disks and partitions
fdisk -l

# Manage a disk
fdisk /dev/sdb

# Then use m to see the menu, and follow the instructions
# to create/delete/manage partitions.
```

## 2. LVM
**L**ogical **V**olume **M**anager (LVM) is a good choice for the times when you
need to create partitions with a size that you might want to change later.

### 2.1. Physical volumes (PV)
```bash
# List all physical volumes
pvs

# Create a physical volume
pvcreate /dev/sdb

# View more info for a certain physical volume
pvdisplay /dev/sdb
```

### 2.2. Volume Groups (VG)
```bash
# List all volume groups
vgs

# Create a volume group
vgcreate vg00 /dev/sdb

# View more info for a certain volume group
vgdisplay vg00
```

### 2.3. Logical Volumes (LV)
```bash
# List all logical volumes
lvs

# Create a logical volume with a name and a fixed size, on volume group vg00
lvcreate --name prometheus_data --size 250G vg00

# Create a logical volume with a size equal too all the free space in the
# volume group
lvcreate --name data2 --extents 100%FREE vg00

# View more info for a certain logical volume
lvdisplay vg00/data2

# Remove a logical volume
lvremove /dev/mapper/vgubuntu-data
```

### 2.4. Resizing
```bash
# umount partition
root@latest-gcc:/# umount /dev/mapper/latest--gcc--vg-home

# Check partition (and I hope it will make it continuous)
root@latest-gcc:/# e2fsck -f /dev/mapper/latest--gcc--vg-home
e2fsck 1.43.4 (31-Jan-2017)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
/dev/mapper/latest--gcc--vg-home: 13342/5799936 files (0.3% non-contiguous), 604675/23186432 blocks

# Resize filesystem inside volume
root@latest-gcc:/# resize2fs /dev/mapper/latest--gcc--vg-home 86G
resize2fs 1.43.4 (31-Jan-2017)
Resizing the filesystem on /dev/mapper/latest--gcc--vg-home to 22544384 (4k) blocks.
The filesystem on /dev/mapper/latest--gcc--vg-home is now 22544384 (4k) blocks long.

# Resize volume
root@latest-gcc:/# lvresize --size -2G /dev/mapper/latest--gcc--vg-home
  WARNING: Reducing active logical volume to 86.45 GiB.
  THIS MAY DESTROY YOUR DATA (filesystem etc.)
Do you really want to reduce latest-gcc-vg/home? [y/n]: y
  Size of logical volume latest-gcc-vg/home changed from 88.45 GiB (22643 extents) to 86.45 GiB (22131 extents).
  Logical volume latest-gcc-vg/home successfully resized.

# Remount partition
root@latest-gcc:/# mount /dev/mapper/latest--gcc--vg-home /home

# ==============

# Umount swap
root@latest-gcc:/# swapoff /dev/mapper/latest--gcc--vg-swap_1

# Increase volume
root@latest-gcc:/# lvresize --size +2G /dev/mapper/latest--gcc--vg-swap_1
  Size of logical volume latest-gcc-vg/swap_1 changed from 2.00 GiB (511 extents) to 4.00 GiB (1023 extents).
  Logical volume latest-gcc-vg/swap_1 successfully resized.

# Recreate swap to use the extra space
root@latest-gcc:/# mkswap /dev/mapper/latest--gcc--vg-swap_1
mkswap: /dev/mapper/latest--gcc--vg-swap_1: warning: wiping old swap signature.
Setting up swapspace version 1, size = 4 GiB (4290768896 bytes)
no label, UUID=433537d3-2ebe-4615-9864-20e31471341c

# Start using it as swap
root@latest-gcc:/# swapon /dev/mapper/latest--gcc--vg-swap_1
```

### 2.5. Problems encountered with LVM
##### 2.5.1. pvcreate - Device excluded by a filter.
```bash
pvcreate /dev/sdb
  Device /dev/sdb excluded by a filter.

# I ran this to find more info about the problem:
pvcreate /dev/sdb -vvv 2>&1 | grep /dev/sdb
        /dev/sdb: Skipping: Partition table signature found

# Indeed fdisk, also showed that the disk label type was GPT:
fdisk -l /dev/sdb
Disk label type: gpt

# I googled a bit for this problem and found that I could use dd to clear the
# partition table. The following command will fill the first MiB with zero bits:
dd if=/dev/zero of=/dev/sdb bs=1M count=1

# This fixed the problem. GPT was gone in fdisk, and pvcreate created the volume
```

## 3. mkfs.*
Create a filesystem on top of a partition (create with fdisk) or logical volume
(created with lvcreate).
```bash
# Create an ext4 filesystem on a logical volume
mkfs.ext4 /dev/vg00/data2

# Create an ext4 filesystem on a partition
mkfs.ext4 /dev/sdd2
```

## 4. Mounting
## 4.1. For the current runtime
Mounting a partition/volume for the current runtime will give you access to it's
filesystem only until the next reboot. At the next reboot, the system will not
automatically mount that partition/volume.
```bash
# Create folder where the filesystem will be accessible
mkdir /data2

# Mount the volume
mount /dev/vg00/data2 /data2
```
## 4.2. Automatically mount at system startup
Mounting a partition/volume at system startup can be done through the
`/etc/fstab` file.
```bash
# Mount a logical volume, by UUID:
echo "UUID=$(blkid -s UUID -o value /dev/vg00/prometheus_data) /mnt/prometheus_data ext4 defaults 0 0" >> /etc/fstab

# Mount a partition using the device file
echo "/dev/sdc1 /mnt/hdd3 ext4 defaults 0 0" >> /etc/fstab
```

## 5. Others
### 5.1. blkid
```
# Get information about a partition / logical volume:
blkid /dev/vg00/prometheus_data
/dev/vg00/prometheus_data: UUID="cc02e486-6698-4da4-a49e-65ea076ca867" TYPE="ext4" 

# Get just the UUID
blkid -s UUID -o value /dev/vg00/prometheus_data

# Get data for a partition
root@srv2:/# blkid /dev/sdc1
/dev/sdc1: UUID="bf2cf6f5-b542-4520-8302-31a07e38a464" TYPE="ext4" PARTUUID="e207e1f3-01"


# Show block storage devices
lsblk -o +MODEL
ls -la /dev/disk/by-id/
lsscsi -v
ls -la /sys/class/scsi_host/host*
ls -l /sys/block
# Remove device from kernel abruptly(even if in use) but remember host index (0)
sudo sh -c "echo 1 > /sys/block/sdc/device/delete"
# Rescan for devices on that port
sudo sh -c 'echo "- - -" > /sys/class/scsi_host/host0/scan'
sudo dmsetup ls --tree
```
