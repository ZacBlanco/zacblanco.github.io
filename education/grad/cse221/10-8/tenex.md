---
layout: page
title: CSE 221 - Operating Systems - Notes on "TENEX, a Paged Time Sharing System for the PDP-10"
permalink: /education/grad/cse221/10-8/tenex/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, rust, multiprogramming, TENEX, PDP-10
description: Notes about TENEX
mathjax: true
---

Question:

> What features in TENEX are reminiscent of features in Unix (a later system)?

### Notes

- Needed a better system which bested their previous systems, DEC 10/50 and the
  SDS 940.
- Minimal change could be made to the processor, none to computation
- Needed service within 6 months

Three broad goal categories

- State of the art VM
    - paged virtual address space
    - multiple processes
    - File system integrated into virtual address space
    - extended instructions
- Good human engineering
    - command language interpreter
    - terminal interface design to favilitate interaction
    - VM functions providing reasonable defaults
    - encourage cooperation among users
- Implementable, maintainable, modifiable
    - modular software
    - debuggable
    - instrumentation for performance

#### Hardware

- Created a "paging device" capable of translating virtual pages into physical
ones.
- may have multiple virtual addresses, but only one physical stored.
- copy-on-write feature to permit read/write sharing of pages.
- per-page status bit
- copied to private area once written
- "core status" table noting when a page was referenced and which processes use a page

#### Processor Modifications

- Add a `JSYS` instruction providing an independent transfer mechanism into a monitor
- transfer from user program to monitor routine in one instruction using the "`JSYS` Transfer Vector"
- JSYS address provides the index to the proper transfer vector entry

Is this the beginnings of "root" privilege vs user-level?

### The TENEX VM

- Operates with 256k of Vmem, 512 word page
- paging hardware traps data not in core memory and transfers it to memory transparently from user process.
- does not make processor IO instructions available directly
- virtual memory map of 512 slots

#### Job Structure

- Job is one or more hierarchically related processes
- tree structure directly related by the ability to control/kill the process.
- communication via memory sharing, direct control, or software interrupts
- enables exec to run user programs and handle faults, service interrupts
- allow programs to block for arbitrary events
- implement invisible debugging

#### Pseudo-Interrupts

- process to receive asynchronous signals from other processes or terminals
- detect sets of unusual conditions, including illegal memory reference, process overflow, EOF,
and data errors.


#### Backcompat

- Interestingly, made compatible with DEC 10/50 programs
- Held on the system ready whenever a 10/50 programs makes a syscall -> load into core
- with paging, only one copy on the system

### Terminal Interaction

- ease of use, autocomplete, and prompting for users
- Ctrl-C to stop a process


### Tenex File System

- uniform access of I/O function
- name as symbolic pointer to data
- file access proection
- tree of max depth 5
- automatic version control
- file extensions
- file access of list, read, write, append, execute
- permissions based on file location relative to directory where it is stored
  (15 bits)
 - directory attached to job of the program is same as owning
 - directory attached to job that is running is same group
 - neither of the above cases
- concurrent access via  "thawed" or "unthawed"

### Monitor

#### Scheduler

- Equitable distribution of CPU time - 1/N per job where N is the total number
  of jobs
- TENEX is interactive - give prompt service to interative requests
- Balance set is the set of jobs with highest priority whose total working sets
  fit in core mem.
- interaction causes placement onto high priority scheduling queue
    - For tenex, priority is based on long-term average ratio of CPU use to real
      time.
    - new priority determined by priority before interaction and length of
      interaction
    - priority decreased at constant rate, increased while blocked.
- short burst of max priority and queue position determined by long term average
  - hopeful to get quick service to very short interactions.

- Admin can increase load by increasing priority based on system fraction F $$ C/T > F $$

#### Core Management

- information provided by status table in page hardware is essential to memory
  operation in tenex
- process reference a page not in core, page trap occurs and core management
  routine is invoked to retrieve the page
  - if a process page fault time is > P_av, a system paramter, then it is below its working set size, and a page is pulled in.


