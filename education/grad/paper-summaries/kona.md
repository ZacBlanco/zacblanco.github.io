---
title: Rethinking Software Runtimes for Disaggregated Memory
type: page
conf: asplos
year: 2021
link: https://people.inf.ethz.ch/omutlu/pub/Kona-Rethinking-Software-Runtimes-for-Disaggregated-Memory_asplos21-AE.pdf
---

Abstract:

> Using virtual memory for disaggregation has multiple limitations including
> **high page fault overhead** and high **dirty data amplification**

- new approach -- use cache coherence instead of VM for tracking application
  memory accesses transparently w/ cache-line granularity.

Introduction

- DC memory utilization is low, generally 65% because it must be provisioned for
  peak periods
- normal servers rely on Vmem system for 3 functions
  - fetching+caching remote data
  - tracking dirty data among cached pages
  - evicting cached pages

>  In contrast, throughout their lifetimes, applications write a small part of
each page, typically only 1-8 cache-lines out of 64

see section 2 for application analysis
- some systems avoid faults and work at smaller granularity, but with
  application changes
- implement w/ cache-coherent FPGAs
- developed tools to simulate and eulate necessary hardware primitives
- reduces write amplification 2-10x

contributions:

- analyze shortcomings of current remote memory runtimes
- propose new approach for disaggregated memory software w/ hardware
  primitives for new caching and dirty cache-line tracking.
- design and implement Kona, the software runtime
- design and implement simulators and emulators for measuring Kona's
  solution


- overhead of virtual memory systems is high when remote page cost is around 3us
- larger page size >> larger dirty data amplification

### Overview and Design

- Use the CPU cache coherence protocol within a server to track reads and writes
  - principles:
    - leverage HW to track memory accesses
    - decouple data movement size from virtual memory page size
    - separate data and control path
- disaggregated allocations handled by a rack controller, allocating at
  a coarse granularity with slabs
- assume compute nodes have memory which is a "cache" for disagg memory
- KLib -- software runtime component for using Kona (transparently)
  - handles 3 main operations: fetch, track, evict
- new hardware primitives for two functions
  - cache remote data (identify what to fetch and cache it)
  - track local data (identify what's been modified and needs to be written back)
  - copy dirty data
- no virtual memory for remote memory

#### Reference Architecture

- implement cache coherence on FPGAs attached to CPU with a coherent
  interconnect
- CPU and FPGA each have their own memories and export fake large Phys ADDR
  space.
  - remote memory backs VFMem
- FPGA cannot track CMem, all data must be in VFMem
- kona replaces page fault with cache misses
  - cache miss triggers remote read
- avoids TLB invalidations and shootdowns otherwise incurred because operations
  occur at the cache miss


#### Failure Model

- application/compute host crashes
- slow/unresponsive network
- disaggregated memory failure




