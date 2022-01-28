
---
layout: page
---

- [paper 1](https://ozlabs.org/~rusty/virtio-spec/virtio-paper.pdf)
- [paper 2](http://zhenxiao.com/read/SR-IOV.pdf)
- [paper 3 ](https://sdn.systemsapproach.org/netvirt.html)



## Questions



- Is virtio a full virtualization or a paravirtualization technique? What's its main benefit?
  - virtio is a paravirtualization technique which unifies the linux drivers that Guest VM OSes need
    to support while still maintaining flexibility and performance. It enables hardware improvements
    to be made as long as the underlying implementation conforms to the virtio specification. This
    frees the hardware designers from worrying about OS or driver support as long as virtio
    interfaces are implemented.
- List at least one limitation of SR-IOV
  - SR-IOV VFs are limited by hardware support. So, the hardware must define the maximum number of
    VFs allowed. Depending on desired VM density, the hardware can be a crucial limitation in
    efficiently packing VMs into a single server.
- What are the similarities and differences between network virtualization and traditional server
  virtualization?
  - Both network and server virtualization end up encapsulating data in some form so that authorized
    processes may access it. Servers do this by utilizing modern hardware features such as Intel EPT
    whereas, networks simply do this by encapsulating virtual network packets with physical network
    addressing. On the other hand, network virtualization usually is accomplished with a combination
    of network protocol changes and programmable switching capability. Server virtualization is
    accomplished through building entirely new pieces of software (or hardware) to isolate resources


## Summary

### virtio: Towards a De-Facto standard for virtual I/O Devices

This paper introduces virtio and the motiviation for building a unifying interface for IO devices.
The drivers help improve the portability of underlying hardware or software improvements to all
guest OSes by exposing a common interface for devices to implement.


### High-Performance Network Virtualization with SR-IOV

This paper discusses the concept of SR-IOV in the domain of networking. It demonstrates how
SR-IOV VFs in a virtualized gues is an improvement over traditional networking IO virtualization
by bypassing hypervisor traps.

### SDN Book: Network Virtualization

This book chapter explains the concept of network virtualization and how newer programmable
switching technology can enable network isolation by encapsulating virtualized packets with
the physical network routing information. The authors demonstrate how network virtualization
makes management and even routing more efficient on the virtual networks with the use of
software switching.
