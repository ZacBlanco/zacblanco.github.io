---
layout: post
title: Creating a Swap File on a Ramdisk with Ubuntu 20.04
author: Zac Blanco
keywords: ubuntu, paging, fault, ramdisk, swap, kernel, support, swapping, page, fault
date: "2021-08-13"
---

For some recent work I was looking to negate the latency of storage accesses of
swapping in order to more easily measure the kernel's swap system overhead
without worrying about the latency of the underlying block device.

Naively, I initially tried with `mount -t ramfs /mnt/ramdisk` and simply tried
creating a swap file on that mount, but after trying something to the tune of
`swapon /mnt/ramdisk/swap.img` I was met with an `Operation not
supported`. Some google-fu led me to the answer. [_`tmpfs` and `ramfs` don't
support the APIs required to act as a device to read and write a swap image_](https://superuser.com/questions/539287/swapon-failed-invalid-argument-on-a-linux-system-with-btrfs-filesystem).

However, it is possible (at least if you're on Ubuntu 20.04 Server) to use the
`brd` kernel module to create a block device at `/dev/ram0` which can then be
formatted with a filesystem swap file that _can_ be used as a ramdisk. Here is
the script:


```bash
#!/usr/bin/env bash

# create the mount point directory
sudo mkdir /mnt/ramdisk
# insert kernel module for /dev/ram0 and allocate 2GiB for swapfile
sudo modprobe -C brd.conf brd
# Format our ram device
sudo dd if=/dev/zero of=/dev/ram0 bs=1M count=2048
# create ext4 filesystem and mount (2GiB)
sudo mkfs.ext4 -b 4096 /dev/ram0 536870912
# mount the block device
sudo mount /dev/ram0 /mnt/ramdisk
# create swapfile
sudo dd if=/dev/zero of=/mnt/ramdisk/swap.img bs=1M count=2048
# setup permissions and enable the swap file
sudo chown root:root /mnt/ramdisk/swap.img
sudo chmod 600 /mnt/ramdisk/swap.img
sudo mkswap /mnt/ramdisk/swap.img
sudo swapon /mnt/ramdisk/swap.img
swapon -s
```

And the file for `brd.conf` for the `modprobe` command contains parameters to
ensure the ramdisk is at least 2GiB (the desired size for my swap file):

```conf
options brd rd_size=2147483648
options brd rd_nr=1
```

Frankly, the latency using a ramdisk directly here was pretty disappointing
compared to an NVMe SSD, but I will potentially explore that issue in a later
post.

```
Tracing 1 functions for "read_pages"... Hit Ctrl-C to end.

     usecs               : count     distribution
         0 -> 1          : 0        |                                        |
         2 -> 3          : 0        |                                        |
         4 -> 7          : 11       |                                        |
         8 -> 15         : 61       |                                        |
        16 -> 31         : 1243     |**                                      |
        32 -> 63         : 8108     |***************                         |
        64 -> 127        : 20933    |****************************************|
       128 -> 255        : 17929    |**********************************      |
       256 -> 511        : 11953    |**********************                  |
       512 -> 1023       : 6        |                                        |
      1024 -> 2047       : 1        |                                        |
```

