---
layout: page
title: CSE 221 - Operating Systems - Notes on "Scheduler Activations; Effective Kernel Support for the User-level Management of Parallelism"
permalink: /education/grad/cse221/11-19/sched-activ/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, OS, fs, unix, scheduler, parallelism, kernel
description: Notes for "Scheduler Activations; Effective Kernel Support for the User-level Management of Parallelism"
mathjax: true
---


> Q: The goal of scheduler activations is to have the benefits of both user and
> kernel threads without their limitations. What are the limitations of user and
> kernel threads, and what are the benefits that scheduler activations provide?


## Introduction

- published 1991
- parallel programs can exhibit perf improvement or degradation depending on
how concurrency is implemented.
- threads are multiplexed across a single processor.
- performance of thread management primitives for kernel threads are typically
  better but cost of kernel threads is high
    - user-level threads are on kernel threads, but suffer an integration
      problem
- programmer has dilemma:
    - employ kernel threads which "work right" and perform poorly
    - employ user-level threads on kernel threads that perform well, but are functionally deficient.
- ideas provide
    - functionality that mimics kernel threads
        - no processor idles
        - no high priority threads waits for a processor while low-priority
          threads run
        - when thread traps to kernel, the processor that the thread was on can
          be used to run another threads from the same or different address
          space
    - performance
        - thread management within order of magnitude of a procedure call.
    - flexibility
        - the user-level part of the system is structured to simplify application-specific customization.
        - application can change thread scheduling policy.
- difficulty lies in information spread between kernel and application address space.
- each application knows exactly how many processors have been allocated to it and has complete control over which threads are running on those processors.
- kernel has complete control over allocation of processors and address spaces
- scheduler activation is an execution context for an event vectored from the kernel to an addr space.


## User-Level threads: Pros and Cons

- cost of accessing thread management operations
- cost of generality
- poor integration in kernel
    - wrong abstraction, since kernel makes most things invisible.
- user-level thread that causes a block, the user-level runtime is now
  responsible for getting something scheduled.
    - OS can't schedule the user-level threads.
- ensuring logical correctness can be difficult

## Effective Kernel Support of User-Level Threads

- OS provides each thread system
    - virtual multiprocessor
    -  kernel allocates processors to address spaces
    - each address space's user-level thread system has complete control over
      threads to run
    - kernel notifies an address space whenever kernel changes the number of
      processors assigned to it.
    - address space notifies the kernel when it wants more or needs fewer
      processors
    - application programmer sees no difference (except for perf) when
      programming
- explicit vectoring of kernel events to the user-level thread scheduler
    - communication between kernel processor allocator and thread system
      structured in terms of scheduler activations
    - scheduler activation has 3 roles
        - execution context for running user-level threads.
        - notifies the thread system on kernel events
        - provides space in the kernel for saving the processor context of the
          activations current thread when thread is stopped by kernel.
    - scheduler activation has two execution stacks
        - one kernel and one user address space
    - distinction between scheduler activation and kernel thread
        - once an activation's user-level thread is stopped by the kernel, the
          thread is never directly resumed by the kernel.
        - instead scheduler activation notifying about a stopped thread.
- 4 kernel-thread system upcalls
    - add processor
    - processor preempted
    - scheduler activation blocked
    - scheduler activation unblocked
- in practice, batches of events are upcalled at a single time.
- most state needed to resume a thread is already in user space.
    - kernel-level info such as register state is saved by low-level kernel
      routines that pass the information upwards during an upcall
- 4 issues in practice
    - thread priorities may cause additional preemptions
    - kernel interaction with application is entirely in terms of scheduler
      activations
    - scheduler activations work properly even when a preemption or page fault
      occurs in the user-level threads
    - user-level thread that has blocked in kernel may still need to execute in kernel mode when IO completes
- Notifying the kernel of user-level events affecting processor allocation
    - thread system only needs to tell the kernel about a small subset of
      possible events.
    - notify kernel when getting more runnable threads than processors, or more
      processors than threads.
    - need to make assumption that thread systems are honest in reporting their
      processor and running thread values
- critical sections
    - user-level thread could get preempted or blocked.
    - prevention and recovery
        - (for dealing with preemption)
        - prevention: need yield entire processor control to user level for a
          period of time
        - identifying required pages in a critical section can be cumbersome
    - recovery-based solution:
        - when an upcall informs thread system that a thread has been preempted
          or unblocked, system checks if thread was executing in a critical
          section
            - if in critical section, scheduled via user-level ctx switch
        - continuing a lock holder ensures that once a lock is acquired it is
          always eventually released.

## Implementation

- processor allocation policy space shares processors
- default scheduling policy is per-processor ready lists in LIFO order
    - scan for work if ready list is empty.
- threads set a flag when in a critical section to allow scheduler to check if
  thread is critical during preemption
- discarded scheduler activations are utilized for re-use to prevent additional
  allocations
- added debugger support

## Performance

- cost of new thread is same as fast threads
- upcall performance is slow
- application speedup not as good as topaz threads on pure CPU
    - common thread operations are more expensive
- when IO involved, it gets better.


## Lecture Notes

- Purpose:
    - solve user-level/kernel-level thread problem
    - new scheduler abstraction
- user-level vs kernel-level threads
    - user-level threads
        - improve perf and flexibility
            - runs at user level, threads managed by lib linked to application
            - high perf thread routines switch similar to procedure call
            - customizable to application
        - OS unaware of user-level threads
            - kernel events hidden from user-level
                - IO, page faults both block the kernel and/or user thread, even
                  if there are other user-level threads that can run
            - kernel threads scheduled w/o respect to user-level state
                - multiprogramming when OS takes away kernel threads - preempts
                  kernel threads without knowledge of which user threads are
                  running
                - may cause poor performance w/ user-level scheduler
    - kernel-level threads
        - OS aware of kernel threads. Integrated w/ events and scheduling
        - high cost to creation
            - high overhead: syscall to invoke thread routine, ctx-switch
            - too general: have to support all applications, adds
              code+complexity
- paper argues that programmers want user level threads
    - cheap parallelism
    - flexible
- scheduler activations
    - pure mechanism that enables user-level to have complete control over
      scheduling policy
        1. vessel for executing a user thread context
        2. mechanism for notifying user level of kernel events
        3. data structure for storing user-thread state in kernel
    - three aspects of design
        1. upcalls OS to user level thread lib
        2. downcalls to user level thread library
        3. critical sections
- each process supplied with a virtual multiprocessor
    - kernel allocates virtual processors to address spaces
    - user level threads system (ULTS) has complete control over scheduling
    - kernel notifies ULTS whenever it changes the number of processors or a
      user thread blocks or unblocks it
    - ULTS notifies kernel when an application needs more or fewer processors
- upcalls --> see notes from reading above
- upcall points
    - add processor, processor preempted, activation blocked/unblocked
    - what happens when an activation blocks
        - old activation blocks
        - OS creates virtual processor, assigns it to process
- downcalls from user to kernel level
    - advantage of scheduler activations is that user does _have_ to tell OS of
      all threads operations with kernel
    - downcalls
        - add more processors
        - processor is idle
- critical section impl
    - thread can be executing in a critical section inside the user-level thread scheduler when blocked
        - poor performance (blocking on lock)
        - deadlock (blocked thread has ready list lock)
    - approach
        - recovery
            - on thread preemption, or unblock, check if in critical section. If
              so, run until critical section finished




https://www.youtube.com/watch?v=MvMJwV7-6XI&feature=youtu.be