---
layout: page
---

- [Hints for Computer System Design](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/acrobat-17.pdf)

## Questions

This course has covered the building blocks of virtualization and the techniques that have developed
since the need for virtualization initially arose. CPU and memory virtualization techniques were
traditionally the main challenge with virtualization. Virtualizing single CPUs is easy. Usually the
hard part of virtualizing CPUs is the virtual memory management because its so tightly coupled to
the CPU hardware. Xen was able to solve this problem with paravirtualization by designing an
virtualized MMU that the guest OS needed to be aware of in order to make faster page translations
with smaller overhead. Later on the CPU manufacturers implemented hardware page table walking to
enable new VMMs like KVM and VmWare ESX to launch any OS without modification and still maintain
performance. Memory management itself has been a challenge and VMware ESX also introduced memory
ballooning to manage page evictions among guest OSes that were managed by a VMM on a single machine.
On top of these traditional virtualization techniques, the course also covered the advancing state
of virtualizing infrastructure, such as the use of containers, which are essentially just an
OS-level isolation mechanism to constrain resource and device access for particular applications.
However, they do not provide as many security guarantees as VMs due, mainly due to the use of the
same OS which has the same page tables. A few different projects exist to try to limit the container
attack surface, but none of them truly achieve the same security boundary that having separate VMs
do. Additionally, the course has looked at the need for extended virtualization techniques to
accommodate IO (disk and networking) and accelerator devices (GPUs) for use in modern cloud
datacenters because they are now becoming just as important to many workloads as CPUs have been for
years.
