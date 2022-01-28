---
layout: page
---

- [paper 1 (first 3 pages)](https://www.vmware.com/pdf/Perf_ESX_Intel-EPT-eval.pdf)
- [paper 2 (first 4 sections)](https://www.vmware.com/pdf/usenix_resource_mgmt.pdf)



## Questions

- List at least one pro and one con for software MMU and for hardware MMU
  - hardware MMU
    - pro: hardware MMUs improve performance by not waiting on the CPU to resolve virtual addresses to physical. No need to
    - con: when implemented in hardware, flexibility is limited and future implementations are limited by the hardware
  - software MMU
    - pro: software MMUs are more flexible , and TLB entries can still be used
    - con: still need to maintain shadow page tables when PTEs are updated
- What is the double paging problem and what caused it?
  - The double paging problem occurs because the VMM page replacement policy may choose a page from
    a guest, but the guest might also select the same page if it is under memory pressure. Thus, the
    page could be "double paged" potentially causing twice the amount of IO.
- What is the benefit of keeping a "hint" entry for each scanned (but unshared) page (as compared to not maintaining anything for the page)
  - maintaining the hint allows us to find potential matches quicker by not having to search for
    another matching page. The hint entry points to a page which has (or had) the same hash at some
    point which the VMM can then be checked if the page content still matches. Without keeping the
    hint you would every page needs to be marked COW which could create overhead for later writes
    when no sharing occurs.

### Summary

#### Performance Evaluation of Intel EPT

The authors explains the EPT feature of newer Intel processors. Their diagrams show how EPT removes
the need for the software MMU to maintain shadow page tables.

#### Memory Management in Vmware ESX Server

This paper explains various techniques of how the VMWare ESX Server VMM deals with the various
problems of memory management with virtual machines. First the explain how the balloon driver
creates more predictable behavior in page reclamation for VMs. They also explain how they save
memory by sharing pages which contain the same content across different VMs using page scanning and
hashing of the page content.
