---
layout: page
---

- [Amazon Nitro](https://aws.amazon.com/ec2/nitro/)

## Questions


- With Amazon Nitro, virtualization functions are mostly offloaded to
  hardware. Do we still need a hypervisor (or an OS)? Can everything
  just run in user space and interact with Nitro cards directly?
  - Nitro cards are mostly focused on providing IO virtualization for devices
  like the network adapters and persistent storage. While it may be possible to
  use the IO devices directly without virtualization there are still challenges
  of memory isolation and protection between VMs which use the same CPU and/or
  motherboard which are not solved by the nitro solution. Thus, hypervisors are
  likely still required to enable memory and other system protection (like the
  FS), but if a user just wants to perform high-performance virtual IO, then a
  nitro card may be a desirable solution.
- Can you think of a drawback of offloading tasks to hardware (i.e., Nitro's
  approach)?
  - one drawback of hardware offloaded functions in general is the lack of
  configurability or elasticity in the case of changes in policy or target
  performance goals. Hardware cannot be reconfigured easily unless it is built
  on a FPGA chip. Otherwise it may cost thousands, or even more likely millions,
  of dollars to design or change a chip even slightly. Thus, software still
  presents a good case in the use of virtualization even though its performance
  is not as good because policy changes can be implemented in a matter of days


## Summary

The Amazon Nitro system introduces hardware-accelerated virtualization in order
to more performantly multiplex system resources inside of the AWS datacenters.
AWS has 3 nitro cars which mainly focus on IO virtualization to provide high
performance sharing of IO resources between IO devices and VMs. The cards
focus on NVMe storage, networking, and system root of trust.
