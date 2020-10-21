---
layout: page
title: CSE 221 - Operating Systems - Notes on "Medusa; An experiment in Distributed Operating Systems Structure"
permalink: /education/grad/cse221/10-20/medusa/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, medusa, distributed, os
description: Notes for "Medusa; An experiment in Distributed Operating Systems Structure"
mathjax: true
---


> Q: What are the three distributed OS structures outlined in the paper, which structure does Medusa use, and why?

**Answer**

Distributed OS architectures

- 3 separate machines, one machine containing all of the core utilities of an OS
    (memory manager, FS, Process Manager) which are run locally to that processor
    - The rest of the processors invoke the utilities remotely.
- Replicate all OS code in every processor, similar to networks
    - req/response for some number of functions
- Split functions among nodes, make requests to different nodes containing the utilities (medusa uses this)


### Intro

- multi-user OS
- 2nd OS for Cm*
- Designed for a few key system attributes
    - Modularity
        - large number of small components
    - Robustness
        - respond in reasonable way to env. changes (e.g. hardware failures,
          workloads, firmwares, etc)
    - Performance
        - structure of the os and abstractions to programs reflect underlying
          hardware


- issues of partitioning and communication
- Two significant characteristics
    - Control structure of OS is distributed
        - Each component is distributed to perform 1 task as a single utility
    - Parallelism is implicit
        - program is a "task force", set of cooperating activities

### Distributed Structure

- system structure
    - Set of compute modules (cm's)
    - number of compute modules behind a kmap (communication controllers)
        - Set of cm's is a cluster, kmaps handle inter-cluster communication
    - no central authority
- performance highly dependent upon local-hit ratio to memory
    - good perf. requires 90%+
- in traditional OSes, users assume all functions of an OS, e.g. memory mnager,
  FS, and process manager are all local to the processor and available
    - unlikely the case in medusa
- if all OS systems resided on a single CM, then the reliability of the entire
system relies on the sole reliability of that CM.
- alternatively, replicate OS code in each CM.
    - OS is too big for each CM. nearly 50% of memory used for OS.
- cache parts of the OS?


- Medusa solution?
    - discard notion of all OS code executed from any point in the system.
    - OS is split into disjoint utilities
    - programs may need to switch processors
    - medusa messaging provides cross-processor execution
    - function invocation via pipes, response on return pipe
    - argue message transfer is logically equivalent to passing control
    - strong separation for protection
    - makes system very modular

### The Task-Force Structure

- a collection of concurrent activities which cooperate on execution of a single
  task.
- maintains a collection of objects allowed to be manipulated by said activities
- access to objects via descriptors protected by descriptor lists
    - page objects
        - access to 16 portions address space of 64k memory (e.g. page is 64k/16
          = 4k)
    - pipes and semaphores
        - manipulated via requests to kmap
    - file control blocks, descriptor lists, task forces, etc
        - implemented by OS utilities
- every task has a PDL (Private Descriptor List) and a SDL (Shared Descriptor
  List)
- Size of FIlesystem task force varies dynamically to provide load blaancing and fault tolerance
    - existing activities overloaded? create new ones to assume load
- always at least to activities in every utility task force

- SDL/PDL task force organization has levels of locality reflecting the Cm*
  hardware organization.
    - PDLs are individual Cm's, whereas SDLs are analagous to clusters.

- implement task force notion as low level structure because
    - OS functions are provided by task forces, same structure should exist at
      the OS level
    - Activities within a task force interact on a fine grain, guarantees need to be made of the task force (e.g. scheduling)

### Overview of Current System

- Kmaps implemented to guarantee consistency of descriptor space, block
  transfers, and operations on pipes and semaphores
    - event system in kmaps to implement a signal/await events.
- kernel in each Cm for interrupts generated

System Utilities

- Memory manager
    - Allocation of primary mem + kmap microcode descriptor list manipulation
- Filesystem
    - a controller for all IO of the system
    - hierarchical fs similar to unix
- Task Force manager
    - creates and deletes task forces and activities
    - simple debugging facilities
- exception reported
    - communicate information about exceptions to relevant parties
- debugger/tracer
    - symbol tables and perf-measurement information of all utilities

### Pipes and Communication

- similar to unix - streams of bytes
- integrity of messages is insured by maintaining byte counts for each message
- only whole messages read from bytes
- identity of sender is included with each message
- overhead of messaging is very important to achieve good perf.
- pause time to prevent context switch

- every processor has a UDL (utility descriptor list) used by activities to
  communicate with utilities
- to invoke, call the kmap with the index of a pipe descriptor on the processor
  (CM) UDL
    - UDL in two groups
        - user or utility activities
        - utility activities (privileged)
- without protected portion of UDL, performance and reliability jeopardized

### Utility Privileges

- utilities execute with a status bit set to allow privileges
    - share address space
    - perform special operations
- Sharing of address space with XDL (external descriptor list)
    - not a new list, but way to access PDL, SDL of another activity.
- rights amplification by utilities to manipulate representations of protected
  types.
    - make special request to kmap
- makes argument for a layered approach when a system is distributed
    - lowest layers must be everywhere


### Internal Utility Structure

#### Deadlocks

- Example situation
    - FS needs to open a file
        - FCB needs to be read into memory (MMU operation)
            - MMU operation needs to perform filesystem IO to replace pages (swap)
                - FS waiting on MMU
                    - MMU waiting on FS
                        - ...

- System utilities need to provide all services within a _class_
- no circular dependencies

#### Coroutines and Multi-Events

- dedicate activities of a utility to provide each class of functionality
- activities are a collection of coroutines
- any activity of a utility is capable of providing all classes of utility
  function
- receive messages from one of several pipes
    - each activity is mpsc, but whole utility is mpmc?
- no synchronization constraints imposed by multiplexing (busy-waiting on locks not permitted)

### Exceptions

- exception may or may not be an error. an unusual occurrence
- activities can specify a handler
- for shared objects, all co-owners participate in handling
- exceptions can be reported internally without being reported externally
- exception handling through **buddy** which is distinct from site of exception
discovery
- emulate trap instructions



