---
layout: page
---


## Introduction

- architecture for a system with a multiprocessor where the physical memory is distributed does not have an obvious implementation
- _shared virtual memory_ problem: virt. addr space shared among many processors in a loosely-coupled system


## Shared Virtual Memory

- Single address space shared by a number of processors
- any processor can access any memory location
- memory map managers implement mapping
  - must also be **coherent** (value returned by read is the same value returned
    by the most recent write)
- centralized and distributed map managers are possible
- system does not require explicit input from users on where to place pages
  before/after reference

## Memory Coherence Problem

- shared VM idffers from shared cache on a single multiprocessor
  - because most is hardware-implemented, much quicker
  - distributing across machines is much slower to resolve conflicts
- two key parameters
  - page size
  - coherence strategy


### Granularity

- overhead of sending 1 vs 1000 bytes is roughly equivalent
- larger page unit size likely results in higher probability of conflict
- right size is likely application-dependent

### Memory Coherence Strategies

- lots of choices to solve coherence..
  - problems of page synchronization and page ownership
- page synchronization
  - invalidation
    - one owner of a page. write fault to a page invalidates all copies of the
      page, writes change
    - after modification, processor "owns" page and may read/write until
      ownership is relinquished
  - write-broadcast
    - writes to all pages synchronously
    - returns faulting instruction
- page ownership
  - fixed or dynamic
    - fixed: page always owned by same processor, more expensive,
      requires hardware support
    - dynamic: ownership may change
  - strategies may be centralized or distributed
    - distributed strategies sub-classified as _fixed_ or _dynamic_
- page table, locking, invalidation
  - algorithms in paper are all implemented as page fault handlers
  - page data structure has following info:
    - page access
    - copy set: processor numbers with read copies
    - lock: synchronize multiple page faults by different processes
  - invalidation: send invalidation request to processor _i_
  - need reliable communication protocol

## Centralized Manager Algorithms

- monitor-like centralized manager
  - data structure with a set of procedures with mutually exclusive access to
    data
  - if not manager, send request for page to manager, get page, reply with
    confirmation
  - manager locks the page, adds requestor to copy set
  - all writes go to central server which invalidates copy set and then returns
    to users
  - change owner as writes go around
  - worst-case page location takes 2 network messages
- improved centralized manager
  - move synchronization of page ownership to individual pages eliminating
    confirmation operation
  - moves information form data structure on manager to each processor
    - requires access, lock, copyset all to move to processors
    - requests are simply forwarded to the correct machine
- distributed manager algorithms
  - fixed distributed manager algorithm
    - every processor given a predetermined subset of pages to to manage
      - difficult to choose an appropriate mapping
      - Mapping function H(p) determines the processor to communicate with
  - broadcast-based manager
    - processor only manages page that it owns, faulting processors send
      broadcast to find the page owner
    - cost of broadcast is too high for large systems (n >= 4)
  - dynamic manager
    - keep track of ownership of all pages in the processor's local page table
    - owner field replaced with "probOwner" --> true owner, or most-likely owner
      - must be maintained for each page over time
    - page owner may change over time. When processor faults, send a request to
      the processor indicated by "probOwner". If the processor is the owner, use
      the same algorithm as centralized owner, otherwise forward the request
      again
    - probOwner changes on all invalidation and read faults
    - theorem: owner requests eventually arrive at the true owner
    - theorem 2: worst case messages for locating an owner is N + K log (N)
    - optimization: broadcast owner after M page faults

## Experiments

- Run parallel computation algorithm across many machines:
  - potentially super-linear speedup if program needs to page to disk
    - increasing total available RAM where page fault < disk fault time causes
      better performance
    - other benchmarks didn't fare as well (e.g. dot product/merge-split of
      merge sort)

- conclusions
  - dynamic manager algorithm perform better than fixed methods
    - number of processors sharing a page for a period of time is small.
  - expect speedups in a shared-memory system
  - page size is still up for discussion.
