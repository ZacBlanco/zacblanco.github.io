---
layout: page
title: CSE 221 - Operating Systems - Notes on "Exokernel; An OS Architecture for Application-Level Resource Management"
permalink: /education/grad/cse221/11-3/exokernel/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, rust, OS, exokernel, micro, kernel
description: Notes for "Exokernel; An OS Architecture for Application-Level Resource Management"
mathjax: true
---


> Q: Compare and contrast an exokernel with a microkernel

## Introduction

- OS can significantly limit HW performance for applications.
- Abstractions for HW discourage domain-specific optimization
- restricts flexibility of application builders
- solve via app level (untrusted) resource management
    - app must implement
    - exokernel is only the bare minimum OS
    - exokernel is merely a multiplexer
- Overarching goal is to separate protection from resource management
- Achieve via
    - secure bindings (bind to machine resources)
    - invisible resource revocation (sharing protocol)
    - abort protocol (break bindings of onccoperative applications)


## Motivation for Exokernels

- Cost of high-level abstractions
    - argue traditional OSes are overly general
    - no single way to create abstraction that is best for all apps
      (hurt performance)
    - abstractions hide information, difficult for apps to implement any
      resource monitoring themselves
    - limit application functionality due to available interfaces
- Only applications know what is best, allow them to make those
  decisions on hardware resource mgmt
- don't need to cross into kernel nearly as often because most management routines are in same address space (library OS)

## Exokernel Design

- programming error in one library OS app should not affect another.
- separates protection and management in a low-level interface
    - tracks ownership of resources
    - guards resource usage and binding points
    - revokes access to resources.
- exokernel specifies the details of the interface
- needs to export hardware, and more. e.g.
    - DMA, CPU, physical memory, disk, TLB, addressing, interrupts,
      exceptions, cross-domain calls
- exokernel only needs to do management in the lightest sense. *fair*
  allocation is not necessary.
- only needs to
    - expose allocation
    - expose names
    - expose revocation
- Secure Bindings
    - protection of resources
    - protection checks involved are expressed in simple operations
    - authorization checks only ad bind time.
    - implemented via some hardware mechanisms, software caching, and
      downloading application code
    - use large software TLB to cache bindings
    - "download code into kernel" - (eBPF-like)?
- Multiplexing Physical Memory
    - exokernel guards access to a physical page by requiring a
      capability be presented by the accessor.
        - use hardware TLB to get virtual-physical mapping, cache some
          mappings in a software TLB
    - memory implementation will vary depending on the hardware.
    - may need to guard page table instead of TLB.
- Multiplexing the network
    - challenging because need to inspect contents to know recipients
    - use packet filters?
        - isolate faults by limiting language runtime
        - use runtime checks
    - exokernel simply implements packet filters
    - compiles packet filters to machine-code at runtime
        - increase multiplexing performance by order of magnitude or more
    - security issue because filters could look at packets not destined
      for application. Need to "trust" packet filter installer
- Downloading Code
     - application specific safe handlers
        - download handlers into kernel for message processing
            - can initiate a new message - reply transmitted immediately
- Resource Revocation
    - need to be able to reclaim resources visibly or invisibly.
    - needs to reveal revocation to the relevant library in order to
      relocate physical names
    - revocation is "a dialogue between kernel and library OS"
    - abort protocol when library OS does not respond
        - option 1: kill entire application
        - 2: break all secure bindings, inform library OS (exokernel method)
    - use a repossession vector when exokernel claims a resource back
    - guarantee each application (library OS) a small number of pages in order
    to prevent reclaiming pages with important information (e.g. interrupt handlers, page tables)


## Experiments

- Compare Exokernel and ExOS (library OS) with Ultrix, a UNIX variant.
- Ultrix is a high-ish performing OS, much better than other OSes
- isolated network env. single-user
- Ultrix measurements are "best time", Exokernel is median of 3 trials.
- Aegis uses time slices to share cpu
    - applications can choose how they want to be scheduled
    - sends signal to context switch at the end of a slice
        - after an excess number of slices, destroys context if failed
          to switch in time
- Processor Environments
    - 4 execution contexts
        - exception
        - interrupt
        - protected entry
        - addressing
    - execution context define a process.
- Exceptions
    - steps:
        - saves 3 scratch registers into agreed-upon "save area"
        - loads exception PC, last virtual address to fail translation,
          and cause of the exception
        - use cause of exception to perform indirect jump to application specified PC where execution resumes
    - Aegis can dispatch an exception in 18 instructions
    - not using mapped data structures so it does not need separate kernel TLB
- Address Translations
    - TLB Miss:
        - aegis checks segment of virtual address
            - if within standard user segment, exception is dispatched
              according to application
            - if in 2nd region, checks if guaranteed mapping, if so,
              installs the proper TLB entry and continues
            - otherwise, forward to application.
        - application looks up virtual address in PT and
            - if address is not allowed, raises an exception (segfault)
            - if mapping is valid, create TLB entry and invoke
              appropriate routine
        - checks that given capability corresponds to the access rights
          requested
          - if so, mapping is installed in TLB
          - otherwise, error is returned.
    - overlay hardware TLB with software TLB
- protected control transfer
    - synchronous and asynchronous transfers in Aegis
    - asynchronous is the remainder of current time slice to callee
    - synchronous donates remainder and all future time slices until return
- Packet filter
    - most perf comes from dynamic compiling of filters

## Ex OS

- implements pipes and communication quickly
- small VM system (no swap, simple page table)
- ExOS+Aegis slower at protection/unprotecting larger contiguous regions
  than Ultrix.
- Application-specific Safe Handlers (ASH)
    - downloaded code into kernel for packet processing (ExOS side)
    - 4 abilities
        - ASH controls where messages are copied, eliminating additional copies
        - dynamic integrated layer processing (integrate simple data manipulations and checksumming algorithms)
        - message initiation (send new messages)
        - control initiation (general computation)


## Related Work

- Hydra tried to separate mechanism from policy
    - in exokernel, sometimes mechanism _is_ policy?
- exokernel removes mechanism wherever possible
-
