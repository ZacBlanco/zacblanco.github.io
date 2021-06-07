---
layout: post
title: Migrating Ubuntu 20.04 with LVM Partitioning
author: Zac Blanco
keywords: ubuntu, disk, lvm, gparted, parted, linux, partition, disk, migrate, 20.04, 18.04, HDD, SSD, smaller, migration
permalink: /blog/migrating-ubuntu-lvm-partioning/
date: "2021-06-06"
---

We were running some experiments on a local cluster of machines that were
originally installed and configured using the Ubuntu 20.04 Server live CD. After
brief investigation into some performance issues we found that the operating
system (and rest of the system software) was installed on a spinning drive
with performance less than 1/5 of SSD on the system.

We didn't want to re-install the OS and software because there was a lot of
configuration we had to do to the operating system due to some specialized
hardware we were using. So, we decided to opt for a disk migration instead. I
had done some OS migrations years ago, so I figured moving the OS partition for
Ubuntu should be straightforward. I'll preface the rest of the post saying that
the experience wasn't _unpleasant_ but there was little documentation that I
could find on the internet related to newer versions of Ubuntu. Everything
seemed to be for 12.04, 14.04, etc.

I'm writing this post to hopefully help out some other poor lost souls who might
need to migrate their operating system installs from one disk to another (and
save some time in the process) that were installed with an LVM partitioning
scheme.

### Setup

- OS initially installed on a 1TiB drive (`/dev/sdb`)
- Unpartitioned/formatted 512GiB SSD (`/dev/sda`)
- Ubuntu 20.04 installer used with all default settings

The Ubuntu OS installer creates 3 partitions when installing the OS to a single
disk and when the LVM partition install is used

- `/dev/sd*1` --> mounts to `/boot/efi`
- `/dev/sd*2` --> mounts to `/boot`
- `/dev/sd*3` --> mounts to `/`


**The goal is to completely move the boot and data partitions from `/dev/sdb` to
`/dev/sda`** and then finally be able to boot with `/dev/sdb` completely wiped
and formatted.

### Instructions

I used the [`gparted`](https://gparted.org/livecd.php) live CD image to boot
into the OS. Once booted with gparted:

- Make sure all partitions on the target drive (SSD) are removed and the disk
  is entirely blank
- use gparted to clone the partions mapped to `/boot` and `/boot/efi` to
  their respective locations. In other words copy+paste `/dev/sdb1` to `/dev/sda1`
  and `/dev/sdb2` to `/dev/sda2`
- Shrink the lvm partition so that it can fit on the new disk
  - right click the LVM partition -> `deactivate`. Then, shrink the partition so
    that it can fit on the new drive, assuming that your target drive has enough
    free space.

These boot partitions and resizing should be quick. Next, we need to move the
main data partition. Unfortunately, Gparted doesn't support the copy+paste of
LVM-based partitions, so we need to use the command-line. I followed some
[instructions from
askubuntu](https://askubuntu.com/questions/161279/how-do-i-move-my-lvm-250-gb-root-partition-to-a-new-120gb-hard-disk)
in order to create a new LVM partition and move the data to the SSD.

```bash
# We need to create a new partition on the new SSD
# Create the new partition in the shell with "n" using the defaults.
# "w" to write
fdisk /dev/sdNEW

# Once the new partition is created, add it to the logical volume,
# move the data, and remove the old one. "ubuntu-vg" is the volume
# group name that the installer gave to the LVM volume
pvcreate /dev/sdNEW3
vgextend ubuntu-vg /dev/sdNEW3
pvmove /dev/sdOLD3
vgreduce ubuntu-vg /dev/sdOLD3
```

Finally, use gparted to remove the `boot` flag from the partition on the old
drive and add the boot flag to the copy of that partition on the new drive.
After all these steps you can reboot without the gparted live CD.

At this point, the OS should be able to reboot into the OS successfully. However
we never deleted or reconfigured the boot process, so if you run `lsblk` I saw
that the HDD `/boot` and `/boot/efi` partitions were still mounted.

```console
$ lsblk
NAME                      MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda                         8:0    0 447.1G  0 disk
├─sda1                      8:1    0   512M  0 part
├─sda2                      8:2    0     1G  0 part
└─sda3                      8:3    0 445.6G  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0   200G  0 lvm  /
sdb                         8:16   0 931.5G  0 disk
├─sdb1                      8:1    0   512M  0 part /boot/efi
├─sdb2                      8:2    0     1G  0 part /boot
└─sdb3                      8:3    0 445.6G  0 part
sdb                         8:16   0 931.5G  0 disk
```

To fix this, unmount and delete the partitions, then mount the new partitions
(they should contain the same data) and then run `grub-install` and
`update-grub`

```bash
sudo umount /boot/efi
sudo umount /boot
# remove the partitions with "d" and finally a "w"
sudo fdisk /dev/sdOLD

sudo mount /dev/sdNEW2 /boot
sudo mount /dev/sdNEW1 /boot/efi
sudo grub-install
sudo update-grub
```

After running all these steps and a `reboot`, our `lsblk` shows the correct
output, and a free HDD that we can format again use to store more data.

```console
$ lsblk
NAME                      MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda                         8:0    0 447.1G  0 disk
├─sda1                      8:1    0   512M  0 part /boot/efi
├─sda2                      8:2    0     1G  0 part /boot
└─sda3                      8:3    0 445.6G  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0   200G  0 lvm  /
sdb                         8:16   0 931.5G  0 disk
```

