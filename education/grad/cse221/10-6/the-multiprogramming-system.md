---
layout: page
title: CSE 221 - Operating Systems - Notes on "The structure of 'THE'-Multiprogramming System"
permalink: /education/grad/cse221/10-6/the-multiprogramming-system/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, rust, THE, dijkstra, multiprogramming
description: Notes for 'THE'-multiprogramming system
mathjax: true 
---

Question to answer:

> Dijkstra explicitly states their goals for the THE operating system. How do
> these goals compare to, say, Microsoft's goals for the Windows operating
> system? Why do we no longer build operating systems with the same goals as
> THE?

_Answer_


- Goals are different, but built on same underlying technology.
- Computers are used differently than before, so we don't have the same goals.
    - previously - mainframes with many, many terminals running user programs/inputs to solve a problem
    - now - _most_ systems run with a single user attached, but many processes

Goals of dijkstra ([see notes below](#goals))

Goals of MSFT
- Responsive user applications (reactive based on peripheral input)
- background processes to make user experience better (window/display manager),
filesystem search indexing, antivirus?

While the goals of the two systems are seemingly very different, Micrsoft's windows
builds atop a similar kind of system to THE. We've moved passed the multiprogramming system (single CPU, shared among many processes), to a "multi-multi" programming environment (many CPU cores for many processes). The
work of modern operating systems don't have the same goals because (1) the underlying hardware is vastly improved and (2) we use systems differently today
than we had before, sometimes in ways that actually build upon the abstractions in
THE (background processes to make better UX).

Notes

- Dijkstra defines a multiprogramming system as "a system where all activities
  are divided of a number of sequential processes placed on various hierarchies"
- Paper is divided into section of "reporting the progress of building THE as well as describing the challenges they are facing while building it.

**Hardware**

- memory access - $$2.5\mu s $$ access for 32k of addressable mem
- "disk" - 512k of space, 1024 words per access - access time $$40ms$$
- indirect addressing mechanism
- sound system for commanding peripherals/controlling interrupts
- potentially large number of "low capacity channels" - 3 paper tape readers at
  1000 chars/sec.


> The goal of the system is to smoothly process a continuous flow of user
> programs

##### Goals

- reduce turn-around time for small programs
- economic use of peripheral devices
- automatic control of backing storage
    - qq: was this not integrated before? How did systems integrate storage
      before this?
- economic feasibility to use the machine for applications which only the flexibility of a general purpose computer as needed. i.e. it should be cheap
to use for applications which need to perform simple computations

- _not_ intended to have multiple users access at the same time

### Progress report

- a number of minor mistakes, two major
 - major 1: payed too much attention to "a perfect installation"
 - major 2: built the system without the thought of debugging.

He argues that debugging a system like that is complex and its hard to
distinguish bugs from hardware issues - so they were very careful during
implementation by first designing and proving feasibility of it before building
in order to admit exhaustive testing.

### System Structure

 **Storage Allocation**

 - core pages and drum pages for "in memory" and "disk"
 - "information units" to track types of pages. termed _Segment_
 - segments have an independent id mechanism where the # of possible segment IDs is much larger than the total number of pages (virtual memory?)
    - Segment identifier gives fast access to a segment variable whose value
     denotes whether a segment is still empty or not, and if empty, which page
     or pages it can be found in.
    - if a segment resides in a core page needs to be dumped to a drum (disk), there is no need to return the segment to the place where it initially came from. **The minimum latency time to access the drum is used to store the page**  

**Processor Allocation**

- only the succession of states in the computing system for a process has
  meaning - not the timing at which the states progress.
- processes can run sequentially - mutually exclusion of one another to cooperate (share) the processor - delays progress of one or more process temporarily.

**System hierarchy**

- level 0 - responsibility for processor allocation, controlled by timer
  interrupts
    - Prevents single process from monopolizing the system.
- level 1 - the segment controller, synchronized to the drum interrupt, cater to
  bookkeeping results from the backing store (drum)
- level 2 - "message interpreter", takes care of peripheral keyboard input via
  an interrupt. Output of the device is sent to the teleprinter.
    - a process who opens a conversation identifies itself - the operator must
      also be able to identify a running process if they want to open a
      conversation
- above level 2, each process sees itself as having its own private console -
  however, it is merely an abstract achieved via mutual synchronization.
- level 3 is processes associated with buffering of input and unbuffering of output streams.
- level 4 is user programs
- level 5 is the operator

### Design Experience

- Longest time is conception to think of all possible states?
- tested state by state with level 0 hierarchy testing first, then 1, 2, etc..
- building test programs was an difficult.


### Appendix: Synchronization Primitives

- Synchronized of processes done via semaphores (atomic integer).
- PV operations on semaphores - P-op decreases, V-op increases, if val < 0,
  process is put on a waiting list