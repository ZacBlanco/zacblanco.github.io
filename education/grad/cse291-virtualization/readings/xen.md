---
layout: page
---

- [Xen and the Art of Virtualization](https://www.cl.cam.ac.uk/research/srg/netos/papers/2003-xensosp.pdf)

## Questions

- Why can Xen allow guest OS system call handlers to be accessed directly
  (without any ring-0 Xen involvement) but not guest page fault handler?
  - the os system call handlers can be accessed directly because their behavior
  when executed outside of ring 0 is the same. On page faults, the CR2 register
  can't be read from to when outside of ring 0, so the page fault handler needs to
  write to an extended stack frame instead.
- What's the benefit of using asynchronous event notifications from Xen to a VM?
  - The asynchronous notifications can alert the guest to a small variety of
  events such as IO completions that essentially act as "virtual" hardware
  interrupts. This makes the implementation easier. Additionally, for IO,
  requests can be batched together in a single call for the case of IO delivery.
  It is also a generic mechanism which can span many architectures.
- What goals of Xen are not valid or less valid in today's cloud environments?
  - Xen assumes that system designers want knowledge of the physical and/or
  virtual devices that the OS has access to for correctness and performance. I
  would argue in the modern cloud era, most applications designers accept that
  virtualization and sharing has some performance drawbacks, but the costs are
  not high enough to make it worth writing applications with access needs to
  physical hardware. Additionally, paravirtualization in today's datacenters is
  likely not necessary since x86 designers have introduced hardware
  virtualization extensions that essentially render parts of the Xen obsolete
  because they improve the performance of virtualized systems.

## Summary

In this paper, the authors introduce Xen, a paravirtualized hypervisor. The
authors present their reasoning for paravirtualization and show that with minor
modifications to a few operating systems (Linux, Windows XP) and a well-thought
out design for the rest of the hypervisor (e.g. using simple ring buffers for IO
requests) they can achieve higher performance across a number of workloads that
is not possible without Xen.
