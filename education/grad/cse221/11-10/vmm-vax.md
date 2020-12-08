---
layout: page
title: CSE 221 - Operating Systems - Notes on "Virtual Memory Management in the Vax/VMS Operating System"
permalink: /education/grad/cse221/11-10/vax-vmm/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, OS, vax, vms, vmm, virtual, memory
description: Notes for "Virtual Memory Management in the Vax/VMS Operating System"
mathjax: true
---


> Q: The paper states, "VAX/VMS, then, is a collection of procedures that exist in the address space of each process." Explain in your own words what this statement means.

Answer:

## VAX-11 Hardware

- published March 1982
- basic entity in VAX is the process
    - each process has a byte addressable virtual address space, divided into 512 byte pages.
        - 21 bit virtual page number
        - 9 byte offset.
    - page is the basic unit of mapping and protection
- upper two bits divide virtual address space into a number of regions (4)
- low half of the address space (last bit = 0) is known as the system space, shared by all processes in the system
    - only half system space is used in current arch
    - other half is simply reserved
- the upper address of the first half address space is process-specific control region, the lower portion is program-specific.
    - program region (p0) grows upwards toward control region
    - control region (p1) grows downwards toward program region.
- each region defined by page table entries (PTEs)
    - PTE contains
        - valid bit (31)
        - protection field (30:27) indicating required privilege
        - modify bit (26) indicating write has occurred to the page
        - field use by OS (25:21)
        - physical page frame number (20:0)
- P0 and P1 page tables for every process are located in system space
  section of the address space.
    - base registers for p0 and p1 can be paged because they are in
      virtual memory
- Hardware provides a translation buffer for caching virtual to physical
  translations.
    - two sections
        - system translations
        - process translations
    - only process translations need to be flushed.
- VMS OS is shared implicitly via TLB
- user calls OS routines like any procedure
- prevent access to 1st page to catch program errors executing on null pointers.

### Memory Management Implementation

- concerns:
    - effect one a heavily paging programs in the system
    - high cost of program startup and restart time in VM environments
    - increased disk workload due to paging
    - processor time consumed by searching page lists in the OS
- divide into pager and swapper
- pager is OS construct that runs as a consequence of a page fault
- swapper responsible for loading/storing pages
- page replacement on global placement bad for other processes
    - process local page replacement
    - limit on physical memory of a process
    - FIFO replacement for selecting page from resident set.
    - set reference bit whenever referencing a page
- pages when being removed will look at modify bit to determine whether
  page can be added to the free list or modified list (written later)
- page fault time is ~200us
- free list helps reduce fault rates]
- delays modified page writes
    - modified page list acts as a cache of recently removed pages
    - if a page on the list is referenced, it can be returned at low cost
    - when a write request is performed, many pages written at once
    - pages can be arranged on paging file so that they are clustered
    - because writes are delayed, many page writes are entirely avoided
      because either the pages are faulted and/or program terminates.
- paging in system space:
    - in system space, paged and non-paged code and data.
        - once the system is full, system page faults cause pages to be removed from system RSS and placed on a free or modified page list.
            - exception is the process page tables
            - process page table is added to process private resident
              set and not eligible for removal from resident set as long
              as it contains valid PTEs
- swaps can move entire processes, not just pages.
    - keep highest priority processes resident
    - avoid typically high page rates that occur when resuming a process.
- entire process written to swap when removed from memory
- process not in memory becomes able to execute, read in pages from the swapper
- will not load a process to execute unless there is sufficient physical memory for the entire resident set.
- page membership of swap and memory is identical after process is read back in.

## Lecture Notes

- VAX: Virtual Address Extension
- VMS: Virtual  Memory System
- VAX and VMS were very influential to today's systems
    - powerful minicomputer and affordable by universities.
    - UNIX ported to VAX
- VMS
    - de-facto virtual memory system for unix
    - cutler one of VMS project leaders
    - Bill Gates hired cutler to design and develop NT kernel
    - many aspects of NT kernel have shades of VMS
- Main point:
    - mechanisms and policies for VM system of minicomputers that supports a
      wide range of application requirements
        - real-time
        - batch
- Address Space
    - four regions of VAX/VMS address space
        - P0, P1, Sys, Res (see notes from above section for additional info)
- OS in address space of every process
- OS can directly access user code and data
- page out OS structures
- is the OS actually copied though?
    - no, physical page just mapped to multiple virtual addresses
- VM Hardware
    - page table
        - page tables don't have holes
        - implications for address space usage:
            - cannot efficiently support sparse address spaces
            - multiple stacks for threads, shared mem, shared libs
- TLB
    - 1 or 2 levels in some modern systems
- user vs kernel
    - user and kernel page tables are similar
    - how do they differ?
        - user page tables in kernel address space
        - kernel page tables exist in physical memory space
    - mapping for kernel data structure cannot be paged out.
    - user page tables may be paged
    - kernel page tables cannot be paged out
- command interpreter of VAX/VMS
    - command interpreter is a part of each user's addr space
    - windows/unix command interpreter is not like this because we use `fork`
- 3 techniques for VM performance
    - local page replacement
    - page caching (free list mechanism)
    - clustering
- local page replacement - why?
    - each process gets a max RSS
    - when reaching max, replace its own set of pages
        - isolation, prevent single process from using too much mem
- Page replacement policy
    - FIFO
    - suffers from Belady's Anomaly (more physical mem results in more page
      faults)
- what is page caching?
    - page caching
        - similar to hardware's victim cache for L1/L2
            - keep tiny tiny cache of recently evicted cache lines in case
              needed again
    - two lists
        - free list: clean pages evicted from a process resident set
        - modified list: dirt pages
    - a fault to a page in either list brings is back into resident set w/o disk
      op
    - 50% of faults were to pages on the list
- swapper swaps entire process and data structures to disk
    - maintains highest priority processes in memory
    - need enough physical memory for resident set to be swapped in
- page clustering
    - small pages: 512 bytes
        - PDP-11 compatibility..
    - inefficient for IO
    - want to do IO in large chunks, clustering emulates large pages
    - read: demand paging from executable file, reads many pages at same time
    - write:
        - write chunks of modified pages at ~100 at a time
        - try to write contiguous
        - cluster reads on subsequent faults
