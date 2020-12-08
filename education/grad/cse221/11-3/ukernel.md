---
layout: page
title: CSE 221 - Operating Systems - Notes on "The Performance of Micro-kernel-Based System"
permalink: /education/grad/cse221/11-3/ukernel/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, rust, OS, microkernel, micro, kernel
description: Notes for "The Performance of Micro-kernel-Based System"
mathjax: true
---

> Q: Compare and contrast the L4 microkernel with the RC4000 Nucleus and
> the HYDRA kernel in terms of their goals to provide a basis on which
> higher level OS functionality can be implemented.

Answer:

## Introduction

- published in 1997
- pure microkernel only supplies memory addressing, threading, and IPC
- goal is to show microkernel is usable in practice
- goal are to show that microkernels are usable without huge performance hits
- also adapt Linux! to microkernel
- show extensibility check whether abstractions are reasonably
  independent of hardware


## L4 Essentials

- Two basic ideas: threads and address spaces
    - construct recursive address spaces created by user-level applications
        - construct via grant, map, unmap
        - dynamically associate pages with process or threads
        - IO treated as parts of address space to be mapped and unmapped
    - interrupts and traps are synchronous to raising thread
    - small address spaces can be used up to 512MB for hardware support. (only
      on Pentium). after 512MB switch back to "normal" 3GiB mode
- Works on Pentium originally, ported to Alpha and Mips processors
    - same APIs, but different implementations

## Linux on L4

- Build Linux on the L4 Kernel
- newer linux versions on top, since there not many any optimizations made
- moving to new platfom only requires changing the device dependent part, which
  is mainly device drivers
- L4Linux has full binary compatibility w/ standard Linux
- keep full page tables from hardware inside L4 - and thus, Linux needs to
  maintain its own logical tables.
    - doubles memory consumption of page tables
- L4 maps hardware interrupts to messages which are then received by kernel
  threads
    - top half interrupt executes, then bottom halves
- Each linux process implemented as an L4 task (address space + threads)
- Ways to make syscalls implemented implemented via RPC
    - modified version of libc (shared lib, .so) uses l4 ipc primitives to call the server
    - modified version of libc.a
    - user-level exception handler emulates native syscall traps instruction by
      calling a specific routine in the modified libc
        - this one necessary for full binary compatibility
- linux server threads operate in a tiny address space simulating a tagged TLB
- linux always maps current user address space into kernel space, copy in/out
  operations are done as normal memory copies. Translation done by hardware.
- L4 uses physical copy in/out to exchange data between kernel and user
  process.
- transmit signal to another thread by modifying the stack, stack
  pointer, and instr. pointer
- l4 handles linux scheduling, so not much to do
    - when syscall completes and reschedule bit is not set, put process
      back onto the CPU
- TLBs are critical to keeping performance for memory hits
    - simple hello world program on linux is 800K total and needs 32 TLB
      entries
    - need smaller and more compact address space layouts to avoid conflicts
    to prevent TLB misses.
- Mistake on mapping all of current process address space into kernel address space.
    - required copies of kernel thread data
    - resulted in requiring synchronization; non-trivial and error-prone
    - use single address space instead of mapping, downside is that virt address needs to translate via software to hardware address for copyin/out operations

## Compatibility Performance

- want to answer
    - what is penalty of using l4 linux compared to native Linux
    - does perf of the underlying microkernel matter?
    - how much does co-location improve performance?
- significantly different results for operations taking 1-5 microseconds
- Generally, perf of L4Linux for microbenchmarks is 2-3x number of
  cycles of native linux (compared to 5-10x+ for other microkernels).
- Macro benchmark (compile linux kernel) was only 6.3% worse


## Extensibility Performance

- ok so you can emulate the UNIX API, so what?
    - what is added value?
    - allows "specialization" and "extensibility"
- Ex: Unix Pipes vs RPC
    - l4 kernel nearly doubled pipe performance with their impl and reduced
      latency from 29 to 22 microseconds
- Virtual memory operations
    - fault times:  time to execute user instruction page fault, notification of
      pager, mapping the page, and completing the instruction
    - significant performance improvement
    - reimplemented fault handlers by associating a specialized pager to thread
      executing
- Cache Partitioning
    - 64x64 matrix multiplication interrupted every 100 microseconds
        - takes 96.1ms to complete due to cache inconsistency
    - partitioning pages into 2nd level cache exclusively
        - avoids secondary cache interference, worst case time is reduced to
          24.9ms
    - good for real-time system which needs predictable performance


## Lecture notes

- Types of kernel designs
  - monolithic
  - microkernel
  - hybrid
  - exokernel
  - _virtual machine?_
- windows is a hybrid kernel
- linux is monolithic
- monolithic
  - all OS services operate in kernel space
    - hard to extend for special applications
    - heavyweight for special applications
    - many millions lines of code
    - can be difficult to maintain
  - multics, unix, bsd, all monolithic
- microkernels
  - minimal approach
    - ipc, vmm, thread scheduling
  - everything else into userspace (network, fs, UI)
  - benefits:
    - more stable, less service in kernel space
    - more extensively
  - disadvantages
    - many system calls, context switches
  - e.g.: Mach, L4, AmigaOS, Minix, K42
- hybrid kernel
  - combine both monolithic and microkernel
    - speed and design of monolithic
    - modularity and stability of a microkernel
  - still similar to monolithic
  - e.g. Window NT, NetWare, BeOS
- exokernel
  - follow end-to-end principle
    - very minimal
    - fewest HW abstractions
    - allocates physical resources
  - disadvantages
    - more work for application developers
  - e.g. nemesis, ExOS
- performance measurement
  - when checking perf on OS, need to know hardware, esp. when dealing
    with microbenchmarks
    - caches matter
    - TLBs matter
    - page tables matter
- goals of this paper?
  - microkernel based systems perform competitively with monolithic kernels
  - pure microkernels _can_ be made as efficient as hybrids
  - specialization, extensibility
- what is a microkernel?
  - ukernel exports and implements a small number of abstractions
  - some form of scheduling vmm, IPC
  - two variants of microkernels
    - Mach "ideal": separate user-level server processes, one per subsystem
    - L4, Mach actual: OS in one user-level process
- L4 implements threads, address spaces, IPC
  - address spaces are recursive
    - management of spaces handled by pagers
    - processes can delegate portions of address space to others
- L4Linux - linux runs as a user-level server process
  - kernel modifications of arch-dependent modules; otherwise, unmodified
  - syscalls are messages between user processes and linux server
  - memory managed by mapping physical memory to linux
    - 1-1 physical memory occupies lower address range of kernel address space
  - applications binary compatible by using shared libraries
    - shared libs, emulation layer between linux API and linux server
  - trampolines
    - syscalls in statically linked unmodified linux binaries are sent back to
      the emulation layer.
  - signalling is emulated to process
  - scheduling, simple round-robin
  - Tagged TLBs by matching process tag to TLB entry.
- specialization and extensibility
  - sp and ex evaluation
    - pipes and RPC
  - VM ops (vm faults)
  - Cache partitioning
    - isolated of HW resource (cache lines)
- micro kernel
  - performance issues
  - community (people willing to work on it?)

- ExOS
  - tension between OS taking care of everything, vs getting OS out of the way
    in order to maximize perf
  - different workload scenarios when OS is used
    - Desktop: users interact needs to be responsive and simple to use
    - Servers: likely dedicate entire machine to doing one thing
  - in practice, companies wind up modifying the OS
  - main point of exokernel is to move everything to user-level
    - perf suffers from generality of OS
    - cannot customize/extend
    - OS hides info from applications
  - this OS layer exports hardware resources
  - OS functionality implemented in untrusted OS lib
  - each untrusted LOS customized to application
  - security can be an issue in exokernel(sharing of resources)
  - three function of exokernel
    - resource ownership
    - protection
    - resource revocation
  - exokernel multiplexes resources via allocation/revocation
  - protection ensures correct ownership
  - tracking resource ownership is just some tables.
- mechanism for protection
  - secure bindings
  - decouple authorization from use
  - e.g.
    - TLB entries pre-authorized, maintained in SW cache
    - packet filters authorized and compiled at installation time.
  - techniques for secure bindings
    - HW, protected data sharing
    - SW,
  - is downloaded code a violation of exokernel arch?
    - packet filters, ASH
    - place where pure ExOS breaks down
    - protection missing
      - everything at user level
      - virt. addr space protection across applications
      - but no protection within a virtual address space.
  - revocation
    - explicit request by exokernel to lib OS
    - request gives libOS opportunity to choose page it wants, save only
      registers it needs
    - relies on libOs to coop
    - can use "abort protocol"
      - take resource by force
      - notify libOS what was taken so it can update data structures
