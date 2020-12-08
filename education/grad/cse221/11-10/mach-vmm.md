---
layout: page
title: CSE 221 - Operating Systems - Notes on "Machine-Independent Virtual Memory Management for Paged Uniprocessor and Multiprocessor Architectures"
permalink: /education/grad/cse221/11-10/mach-vmm/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, OS, cmu, virtual, memory
description: Notes for "Machine-Independent Virtual Memory Management for Paged Uniprocessor and Multiprocessor Architectures"
mathjax: true
---

> Q: Why does Mach support copy-on-write, and how does it implement it?

Answer:


## Introduction

- Published 1987
- OS portability suffers from wide variety of ISAs and memory hardware
- UNIX doesn't take into account HW
- portable OS called mach
    - explore relationship between hardwre and software memory architectures
- mach has full UNIX compatibility and extends UNIX notions of virtual
  memory and IPC
    - supports
        - large sparse virtual address spaces
        - COW virtual copy
        - COW and RW memory sharing b/t tasks
        - mmap files
        - user-provided backing store objects and pagers
- works w/o patterning mach's internal memory representation specific to
  a particular hardware

## Mach Design

- Five abstractions
    - Task is an execution environment where threads run
    - thread is the basic unit of CPU utilization
    - port is a communication channel - logical queue for messages
    - message is a typed collection of data objects used to communicate
      between threads
    - memory object is a collection o data provided and managed by a
      server that may be mapped into address space of a task.
- provide indirection - direct processes by sending messages

### Basic VM operation

- mach task possesses a large address space with a number of mappings
  and ranges of addresses.
- task may
    - allocate region of virtual memory on a page boundary
    - deallocate a region of virtual memory
    - set the protection status of a region of VM
    - specify the inheritance of a region of VM
    - create and manage a memory object that can be mapped into the
      address space of another task.
- virtual memory must be aligned to system page boundaries.
- any power of 2 multiple of HW page size.
- copy on write when sharing memory regions
- pages can be
    - shared
        - memory is shared directly
    - copy
        - pages are copied logically, COW semantics apply
    - none
        - not shared at all, not passed to child
- protection bits for "current" and "maximum" protection

## Implementation of Mach VM

- 4 basic structures for VM
    - resident page table
        - table to keep track of machine independent pages
    - address map
        - doubly linked list of map entries describing a map of range of
          addresses to a region of a memory object
    - memory object
        - unit of backing storage managed by the kernel or user task
    - pmap
        - machine dependent memory mapping data structure
- implementation of machine dependent sections per supported piece of hardware.
- physical memory simply treated as a cache for virtual memory, since
  virtual memory may span much larger spaces than is physically
  available
- size of page in mach is not HW dependent, rather only based on
  boot-time parameter (power of 2)
- addresses in a task address space is mapped to byte offsets in memory
  objects
  - carries inheritance and protection bits
  - operations that need to happen on the list: page fault lookups,
    copy/protection operations on address ranges, allocation and
    deallocation of address ranges
- memory objects
    - memory objects track the backing store address
    - reference counter for each memory object
    - each task gets a pager to determine which memory objects need to
      be loaded.
    - access to pager via port
- sharing memory
    - COW operations have two address map objects which point to same
      physical place
    - any write forces a copy of the page
    - memory object created for the purpose of holding modified pages are "shadow objects"
    - add sharing to address maps to quickly find shared pages
- pmap responsible for implementing hardware-level operations
    - pmap module is not responsible for maintaining any info
    - allows greater control over hardware behavior characteristics
- vm information constructed at fault time from the machine-independent data structures.


## Porting the Mach VM

- mach completely self supporting on VAX and IBM in months
- most time debugging compiler and device driver.
- 3 weeks implementing pmap module
- hardest part is where to determine invalidation of TLB
- IBM didn't allow per-task page tables.
    - only one valid mapping per page :(
- variety of different hardware presents many challenges because they
  all are very different.
- cache consistency guaranteed by flushing a TLB.
    - issue with multiprocessors is on task translation from one process
      to another, could not flush the other processor's TLB if task
      switched.
    - solutions
        - forcibly interrupt all CPUs to flush TLB
        - postpone use of a changed mapping until all CPU have taken a timer interrupt
        - allow temporary inconsistency

## Lecture Notes

- CS academic research community does not highlight IBM much...
- IBM RT/PC (RISC Tech. Personal Computer)
    - IBMs early attempt to capitalize on RISC, developed own chips early on
    - led to IBM dev of POWER architecture
        - original machine good, but didn't have great market success
    - led to PowerPC architecture
        - very successful mainstream
        - adopted by apple
        - used in Xbox360+PS3
- Mach research OS very influential
    - later UNIX versions supported some Mach Features
    - MacOS is blend of Mach and BSD
        - originally from NeXTSTep that migrated into Apple
        - Avie Tevanian co-authors, became CTO of apple
    - MSFT NT HW abstraction layer derived from Mach
    - Rick Rashid left CMU to found MSR. Largest CS research org in the world.
- main idea
    - maintain all VM state in machine-independent module
    - treat HW page tables and TLBs as caches of machine-dependent info
    - how is mach virtual address space different from VAX/UNIX in VAS
        - can allocate any region in addr space (large, sparse, VAS)
        - children can inherit regions (COW too)
        - mmap files
        - user-level pagers and backing store
- key DS in mach VM
    - address maps
    - mmemory objects
    - resident PT
    - pmap (physical map)
- resident page table?
    - keep track of resident page in memory
        - linked by memory objects, pmap, address maps
- The memory objects
    - mach memory abstraction
    - represent contiguous block of VM
    - tasks access by mapping object into their address space
    - contains any resident pages and port for backing tore
    - physical memory not allocated until pages are accessed
    - each object has a designated memory manager (or pager)
- COW
    - best solution for efficient implementation of fork
    - when mach told to copy a range of pages, it lets processes share
      copy of each page
- creates shadow objects for fork
- copy only modified page after a write
- shadow object contains only the modified pages.
- shadow objects
    - implicit objects created for COW support
- pmap
    - pmap is hardware dependent code
    - each HW is different
    - little knowledge about mach vm structures
    - acts as a cache of machine-dependent info
- page replacement policy
    - similar to VAX VMS
    - major change is global FIFO pool replacing resident set of all programs
        - supposedly easier to tune
- external pagers
    - pager associated w/ each mem object
    - kernel sends pageout request to user-level pager, it can decide
      which page to swap out
    - if pager is uncooperative, default mach pager invoker to perform
      necessary pageout
    - upside:
        - vm tuning based on application needs
        - consistency among multiprocessors
        - allowed expansion of VM over network
    - downside
        - upcalls from kernel
        - lots of context switching
- locks and deadlocks
    - mach VM relies on locks to achieve access to kernel DS
        - price to pay for parallel kernel
    - to prevent deadlocks, all algorithms gain locks with linear ordering
- summary
    - no hardware ideal
    - usually designed with specific OS in mind
    - when VM features not designed into OS, it can make it more difficult
- interesting anecdote: multiprocessors
    - memory caches are coherent, TLBs are not.