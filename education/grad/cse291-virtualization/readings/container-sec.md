---
title: Understanding and Hardening Linux Containers
type: page
conf: NCC Group Whitepaper
year: 2016
link: https://research.nccgroup.com/wp-content/uploads/2020/07/ncc_group_understanding_hardening_linux_containers-1-1.pdf
---


## Questions

- What types of isolations does Linux containers achieve?
  - the different isolation types of a container are based on the namespaces
    that the kernel provides. The isolated namespaces per container include:
    mounts, IPC, UTS, PIDs, networks, and users. Additionally, cgroups allow for
    the control of hardware resources on a per-container basis.
- Can one Linux container affect the performance of another Linux container on
  the same machine (i.e., performance isolation)? Why or why not?
  - yes, they can affect the performance. A container is essentially just a
    process which has different resource allocations and some amount of
    isolation. This may not prevent the host kernel's CPU scheduler from putting
    two container processes on the same core. If they are eventually scheduled
    to the same core, performance could be affected due to CPU cache
    interference.
- Why do you think containers are less "secure" than virtual machines?
  - containers are less secure because they still share the underlying host
    kernel. In a virtual machine, if root access is obtained, the other VMs and
    processes within the VMM are safer because the hardware is configured within
    for the VMM to use EPT, meaning that getting to other PT or VMM PT is much
    more difficult. Containers don't utilize the additional level of paging
    meaning the memory of other processes and the host kernel is less secure.

## Summary

This article explores the landscape of container technologies in around the
period of 2016. It dives into an overview of containers and how past concepts
from operating systems such as IBM's Plan9 have evolved into today's containers.
The author then goes on to discuess how containers are isolated through
namespaces and control by cgroups.
