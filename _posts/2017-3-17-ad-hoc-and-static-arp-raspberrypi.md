---
layout: post
title: Ad-Hoc Networks and Static ARP entries on Raspberry Pi's
author: Zac Blanco
keywords: research, consensus, ARP, static, ipv4, inet, linux, raspbian, ad-hoc, adhoc
permalink: /blog/research/ad-hoc-arp-entries/
date: 2016-3-17
---

Part of our research this year involves creating and using ad-hoc networks on a linux-based operating system (Raspbian based on Debian Jessie).

The hardware we're working with:

- 5x Raspberry Pi 2 Model B+
- 2x Raspberry Pi 3

Unfortunately ad-hoc network support and implementation seems to be lacking. We've run across a couple networking issues that we were able to resolve with some simple hacks to the system which allowed our network to function properly.

## Ad Hoc Network Support and Drivers

The Raspberry Pi 3 has a wireless chip which is built into the board. The chip uses a broadcom driver called [`brcmfmac`](https://wiki.debian.org/brcmfmac). This driver seems to properly implement support for Ad hoc networks - so our Pi 3's gave us no trouble.

It was really the Pi 2's which we had trouble with. We were using some wireless adapters which utilize a realtek chip and driver in linux. Specifically the driver these chips were using was the [`rtl8192cu`](http://www.realtek.com.tw/products/productsView.aspx?Langid=1&PFid=48&Level=5&Conn=4&ProdID=277). Unfortunately these stock drivers do not properly support ad hoc networks.

The issue seems to lie somewhere with ARP resolution. Basically the driver doesn't know how to properly go from IP-address to the hardware address on the next layer down. This prevents pretty much all communication on the network. Incoming and outgoing.

The issue is resolved by adding a static ARP entries using the `arp` command. We can make these entries persist between reboots (but not changes in interface state changes).

Every node in the network has the following interfaces file with a differnt static IP.

```
auto eth0
allow-hotplug eth0
iface eth0 inet manual

auto wlan0
allow-hotplug wlan0
iface wlan0 inet static
    address 192.168.2.xxx # xxx is a statically assigned number from 0 to 255 for each node
    netmask 255.255.255.0
    wireless-channel 1
    wireless-essid RPiwireless
    wireless-mode ad-hoc
```

In our case each node has an IP address ranging from 192.168.2.177 to 192.168.2.190

### ARP Table

Every interface has its own hardware address that is needed for the low-level communication protocols. If the devices can't resolve the hardware address they will not be able to communicate. That's the issue we're fix with our ARP entries.

If you run `ifconfig` or `ip addr` you can see the HWAddr for each interface.

Run `arp -an` to see your current arp table.

Turns out you can actually provide the system with some static entries in the ARP table and the network using the following command:

    arp -i _ifname_ -s IP_addr HW_addr

Or you can add entries to the `/etc/ethers` file

    IP_addr1 HW_addr1
    IP_addr2 HW_addr2

Then run

    sudo arp -f

`arp -f` will populate the table using the `/etc/ethers` file by default.

Example

Suppose we are on a node A with IP address 192.168.2.190 with HWaddr `3c:ae:3d:cd:02:b4`. Node B has IP address 192.168.2.200 with HWaddr `3c:ae:3d:cd:43:8d`

Then on Node A we run

    arp -i wlan0 -s 192.168.2.200 3c:ae:3d:cd:43:8d

Then if you run `arp -an` you will see

    ? (192.168.2.200) at 3c:ae:3d:cd:43:8d [ether] PERM on wlan0

Then on Node B we run

    arp -i wlan0 -s 192.168.2.190 3c:ae:3d:cd:02:b4

Then if you run `arp -an` you will see

    ? (192.168.2.190) at 3c:ae:3d:cd:02:b4 [ether] PERM on wlan0

Once both nodes have their table populated then try to do `ping 192.168.2.190` from node B or `ping 192.168.2.200` from node A and the pings should now be returning with reasonable latencies.

### (More) Persistent Entries

The issue now become when we had a large network these tables needed to ALWAYS be populated or else communication would fail. The ARP entries do NOT persist after reboot or networking service restarts.

The easiest way to make entries persist (at least through reboots) is to use the `/etc/rc.local` file which is a script run at every boot. This allows us to add the entries into the table every time we boot. If the entries get erased or we need to change the interface configuration this means we just have to reboot. On a console-only raspberry pi this process only takes about 15 seconds so we settled for it.

We chose to add all of our entries to the `/etc/ethers` file so we didn't need to continuously type the commands. Once the `ethers` file was prepared we then edits `rc.local`

`sudo nano /etc/rc.local`

Just before the `exit 0` line we added `arp -f`

```
arp -f

exit 0
```

Great! Save the file and reboot (`sudo reboot -h now`). Then after rebooting and logging in type `arp -an` and all of the entries should be present.

This allowed us to get the ad hoc networks working using the `rtl8192cu` driver on linux.
