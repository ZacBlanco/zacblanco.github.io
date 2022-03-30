---
layout: page
---

- [KVM: The Linux Virtual Machine Monitor](https://www.kernel.org/doc/ols/2007/ols2007v1-pages-225-230.pdf)
- [QEMU: A Fast, Portable, Dynamic Translator](https://www.usenix.org/legacy/event/usenix05/tech/freenix/full_papers/bellard/bellard.pdf)

## Questions

- What is the implication of KVM forwarding I/O requests to the user space?
  - forwarding IO requests to userspace allows for fine-grained policies for scheduling I/O between
  VMs, but also implies some amount of overhead is added when the I/O request is forwarded.
- What is the benefit of QEMU first translating the source instructions (guest) into
  micro-operations implemented in C and their compiled object files and then translating the object
  files into the target instructions (host)?
    - By translating into micro-ops, QEMU is able to avoid some of the runtime overhead in
    translating the guest instructions common instruction set, and then into x86 instructions.
    Additionally, translating to a common set of micro-ops makes QEMU an extendable and common
    interface for many other architectures.
- Can you think of some good use cases for QEMU+KVM?
  - Since QEMU can translate arbitrary instruction sets (with the right micro-op translations) it can
  be used to emulate any type of hardware. This might include ASICs, GPUs, TPUs, etc. Coupled with KVM
  this means you can launch virtual machines which could give users an environment that seems like
  they are using a real


## Summary

### KVM: The Linux Virtual Machine Monitor

This paper explores some of the basic challenges with virtualization and explains how the
Linux KVM architects have approached engineering a modern VMM within the Linux kernel. They
dive into how KVM makes use of modern CPU virtualization extensions and the design of
the IO, paging, and interrupt systems.

### QEMU: A Fast Portable Dynamic Translator

This paper introduces QEMU, a piece of software that is capable of translating programs written in
different instruction sets to run on x86-based hardware. The authors describe the QEMU architecture
for "compiling" using their dyngen tool and how they use translation blocks and translation cache to
increase performance when executing micro-ops which are implemented in C code that is capable of
being run on x86 hardware.
