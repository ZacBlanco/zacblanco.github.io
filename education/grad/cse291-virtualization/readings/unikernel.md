---
layout: page
---

- [Unikernels: Library Operating Systems for the Cloud](http://mort.io/publications/pdf/asplos13-unikernels.pdf)

## Questions

- Name one benefit and one drawback of compiling a single-image VM.
  - compiling a single-image VM has the added performance benefit that every
  application can run in userspace. The lack of kernel-userspace translations
  means applications can handle higher throughput. One of the downsides though,
  is the potential lack of debugging tools. Live debugging on traditional VMs in
  the cloud generally have the ability to install and run debugging tools or
  make updates to the environment (e.g. updating a configuration file).
  Unikernels, unless designed with these additional tools built it (increasing
  bloat), would not be as easy to debug or troubleshoot.
- Unikernel runs in a single address space. Give one example of how this design helps improve performance.
  - by running in a single address space, the process avoids needing to switch between kernel and
  userspace programs. The overhead of doing such jumps is high, and unikernels avoid it.
- Comparing gVisor and Unikernels, which one do you think is more secure and which is more lightweight?
  - unikernels are more secure and gvisor is more lightweight. They are more
  secure because any attack against the process, even gaining "root" access
  would not render any useful information since there are no other processes in
  the unikernel to snoop on. gVisor on the other hand, only requires running a
  single kernel to support multiple applications. The kernel and sandbox
  combination means less overhead resources required to manage tens or even
  thousands of applications, since all unikernels need to load the same kernel
  code in order to boot, whereas in gVisor it is shared.


## Summary

Unikernels present a view of library OSes which are "cloud ready" and can be
deployed in present day datacenters while ensuring higher security and less
overhead than traditional VMs. They provide security by "sealing" the VM through
marking page table ranges as non-writable. They also allow applications to
execute with less overhead due to a lack of address space switching from
userspace to kernel space.
