---
layout: page
title: CSE 221 - Operating Systems - Notes on "Xen and the Art of Virtualization"
permalink: /education/grad/cse221/11-5/xen/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, OS, xen
description: Notes for "Xen and the Art of Virtualization"
mathjax: true
---


## Intro

- published Oct. 2003
- Xen - high perf resource-managed virtual machine monitor (VMM)
- machines require full isolation
- support variety of OSes
- perf overhead of virtualization should be minimal
- techniques for multi-application workloads on single server rely on OS
  protection mechanisms
- performance isolation is not good on shared systems.


## Approach and Overview

- full virtualization where all HW functionality is exposes has drawbacks
    - never part of x86 design
    - supervisor needs instruction for correct VMM.
    - emulating x86 MMU also difficult (solved with more complexity and
      less perf.)
- Xen uses _paravirtualization_
    - some of same hardware aspects, but not all.
- key design principles
    - support for unmodified application binaries
    - support for full multi-app OS
    - paravirtualization necessary for high perf and resource isolation
    - hiding effects of virtualization from guest OS risks correctness
      and performance.
- key ideas of paravirtual x86 interface
    - memory:
        - segmentation: cannnot instal fully-privileged segment
          descriptors and cannot overlap with top end of linear address
          space
        - paging: guest OS has direct read access to HW page tables, but
          updates are batched and validated by the hypervisor
    - CPU
        - protection: guest OS must be run at lower privilege than Xen
        - exceptions: guest OS must register a descriptor table for
          exception handlers with xen
        - syscalls: Guest OS may install a fast handler for syscalls
          allowing direct calls from application into guest OS, avoiding
          indirection through Xen
        - interrupts: hw interrupts replaced with lightweight event system
        - time: each guest OS has a timer that is aware of "real" and
          "virtual" time
    - Device IO
        - net/disk/etc: Virtual devices are "elegant and simple to
          access": Data transferred with async IO rings. Events instead
          of HW interrupts.

- memory difficult because of the hardware aspects
    - x86 does not provide a software or tagged TLB.
        - guest OS responsible for memory management.
        - Xen uses 64MiB section at the top of every address space to
          avoid TLB flush when entering the hypervisor
        - every time a guest OS needs a new page table (process creation),
          allocates page from memory and registers the page with Xen.
- CPU
    - hypervisor needs to protect itself from the OS.
    - guest OSes need to be modified to run at a lower privilege.
    - most archs only support 2 privilege levels.
    - in x86 ring terminology, hypervisor executed guest OS in ring 1.
    - privileged instructions validated by Xen.
    - exceptions virtualized with tables of handlers.
    - exceptions only checked when presented to Xen
- device IO is presented as fully virtualized with clean device abstractions
    - data transfer to and from domains via Xen with shared memory, async
      buffers, and descriptor rings
    - supports eventing mechanism for delivering interrupts.
        - implemented with a bitmap.

### Cost of porting an OS to Xen

- linux was relatively simple
- XP was more difficult, needs device drivers and also modifications to the PTE.

### Control and Managedment

- Xen attempts to separate policy from mechanism whenever possible
- hypervisor itself provides basic control operations, exported by an interface
  accessible from authorized domains

## Detailed Design

### Control Transfer

- transfer control with hypercalls and events
- domain to xen: _hypercall_
- notifications via events
- hypercall interface allows domains to perform a software trap into hypervisor.
- batches of updates with a queue and single hypercall

### Data Transfer

![](../2020-11-04-12-48-00.png)

- Use a ring mechanism to handle all types of device IO
- generic to handle most types of devices.

### Subsystem Virtualization

- CPU Scheduling: schedules domains based on borrowed virtual time scheduling algorithm
- time and timers: domain virtual time advances only while executing, realtime
  continue based on host machine boot time.
- virtual address translation:
    - hypervisor responsible for managing guest OS virtual page table, since x86
      MMU is in hardware
    - Xen only needs to be involved in page table updates. Register guest OS
      page tables in MMU, but guest OS has read-only access
- physical memory: reservation specified at creation time.
- network: Virtual firewall with virtual interfaces (VFR, VIF)
    - when transmitting packet, guest enqueues buffer descriptor onto transmit
      ring
- Disk:
    - only host has access to physical disks, everything else through virtual
      block device
    - VBD comprises a list of blocks with associated ownership and ACL
      information.
    - translation table maintained in hypervisor between VBD and guest?

## Performance Results

ouch:
> ... sadly are prevented from reporting quantitative results due to the terms
> of the product’s End User License Agreement.

- xen significantly outperforms ESX server.
- 24 of 37 microbenchmarks, xen is very close to bare linux.

## Lecture

- project by university of cambridge
- x86 and ARM
- XenSource, then Citrix
- SOSP'03 speaker was asked how Xen relates to L4 kernel
    - speaker did not know about L4 Kernel
- VMs hot research in 2000s
- Contribution:
    - high performance virtual machine manager that supports strict resource
      control among guest OS
    - hypervisor/virtual machine monitor interchangeable
- VMM
    - exports VM abstraction
    - usually runs multiple OSes with few changes
    - old idea (from IBM)
    - VMWare is most well-known contemporary VM
- Hardware virtualization
    - examples: VMware, Microsoft virtualPC/VirtualServer, Parallels, Xen
- A VMM is just an OS that exposes a virtual hardware interface
- Two types of VMs
    - type 1 VM, runs on raw hardware, no host OS
    - type 2, VMM runs on another (host) OS.
- VMM conceived by IBM in late 1960's
    - popularized by VM/370
- Used for OS debugging, timesharing, support multiple OS.
- why was VMware successful before Xen?
    - market position (linux on windows, or vice versa)
    - EMC acquired VMWare, offered 15-20% of cap to public.
- VMMs today
    - OS development and debugging
    - software compatibility testing
    - software dev./eval
    - Running software from another OS
    - virtual infra for internet services
- Internet Services
    - almost all apps now run inside VMMs on the internet
        - allows anyone to upload new service to the internet
        - advent of EC2, Azure, GCP, etc.

- Xen requirements:
    - isolation
    - heterogeneous OS simultaneously
    - minimal perf overhead
    - strict resource accounting
    - limited modifications to guest OS
    - no change to ABI
    - scale to 100s of OS instances
- paravirtualization vs full virtualization
    - domain0 is a separate VM to provide services.
    - separate domain0 routes IO requests through the device to the host
      devices.
- Possible implementation:
    - complete emulation.
    - this is a slow approach
- protection in traditional OS
    - hardware exposes a user/kernel boundary
        - OS runs kernel mode
        - pocesses in user mode
    - processes safely execute instructions (no emulation)
    - small set of instructions can only execute in privileged mode.
        - e.g. writing to IO, disabling interrupts, etc
- Improve traditional VMM perf
    - treat guest OS like an app.
    - most instruction execute natively on CPU
        - privileged instructions trapped and emulated
        - load store, branch, ALU ops native
        - machine halt, IO instr, MMU manipulation must be translated through
          the hypervisor
- virtualizing the user/kernel boundary
    - guest OS and applications run in "physical" user-mode
        - necessary to trap privileged instructions into VMM
    - for each Virtual machine, the VMM keeps a "software mode" bit.
        - during syscall, switch to kernel mode
        - on syscall, return switch to "user" mode
    - faster implementation on x86 due to 4-ring security support.
- Tracing a read through a FS read
    - read syscall in app
        - goes to guest OS trap handler
            - handles read syscall
                - go to VMM, trap privileged instruction
                    - if kernel mode, emulate virtual disk
                    - else: raise protection violation
        - guest OS finishes read
            - copy data to user buffer
            - return from syscall
    - user app returns.
- x86 somewhat easier to virtualize due to rings
- guest OS runs at ring 1 instead of ring 0.
    - implication is that privileged instruction delegated to Xen either trap or rewrite OS to not use them
    - explicitly need to schedule CPU among guest OSes
    - use BVT (borrowed virtual time)
- x86 hard to virtualize because hardware manages TLB. Software not involved
  with TLB misses and TLB is not tagged
- each guest OS thinks it manages the linear physical memory address
  from 0 to 0xFFFFFFFF
  - each guest OS expects its own page tables, TLB, memory-management unit, etc
  - unlike virtual disk, we can't trap and emulate every memory reference
  - additional complication: protection bits: only some memory accessed
    in kernel mode'=
- options
  - terms:
    - virtual memory, pseudo-physical memory, machine memory
  - bad option 1:
    - insert virtual to pseudo-mapping directly into TLB
    - trap each memory access to return the Virtual to machine memory mapping
  - bad option 2:
    - instead of inserting virtual to pseudo mapping, transparently insert virtual to machine memory mapping
    - this way, all future mem references occur at HW speed.
- how does Xen virtualize?
  - allows HW to access guest OS page table directly
    - swap when switching among guest OSes
    - flush TLB
  - how to prevent guest OS messing up page table and crash the hypervisor and other guest OSes
    - xen directly allocates machine page to guest app virtual page
    - xen validates page tables on installation
      - make sure physical pages are truly allocated by xen
    - Xen write-protects the page table thereafter
    - Xen validates the updates to the page tables during execution
      - need to change OS source directly.
- could use shadow page tables
  - guest OS page tables not directly used by MMU HW
  - hypervisor must keep HW page tables and guest OS page tables in sync
  - required for full virtualization (VMWare), but can avoid paravirtualization.
- balloon driver (from VMWare ESX)
  - mechanism forcing guest OS to give up memory
  - balloon is an artificial memory consumer in a guest OS
  - memory consumed by VMM given to guest OS.
- system c
