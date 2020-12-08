---
layout: page
title: CSE 221 - Operating Systems
permalink: /education/grad/cse221/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, rust
description: Notes for CSE 221 (Operating Systems) at UC San Diego
mathjax: true
---

## Lecture 1 - Intro

Checkpoints - things that we need to be familiar for this class

- What is a privileged instruction. Examples?
- Difference between system call and a function call?
- Give an example of an atomic instruction
- Difference between a semaphore and a condition variable
- Difference between hardware and software-managed TLBs
- What can happen at a memory instruction. Can the CPU directly access the cache to get the data?
- What is an inode?

Use the textbook _Operating System Concept, by Silberscatz, Galvin, and
Gagne_ if you need to brush up on OS concepts.

Objectives for the course

- Understanding system design trade-offs. Previous and modern-day systems.
- Gain experience reading research papers
    - Develop intuition for what question and issues are important, which are not.
- Experience discussing research material
    - Be able to express opinions and arguing points for/against design decisions.

## Lecture 2 - "'THE'-Multiprogramming System" and "The Nucleus of a Multiprogramming System"

- [Notes to THE](10-6/the-multiprogramming-system/)
- [Notes to Nucleus](10-6/nucleus-multiprogramming-system/)

## Lecture 3 - TENEX and HYDRA

- [Notes to Tenex](10-8/tenex/)
- [Notes to Hydra](10-8/hydra/)

## Lecture 4 - Protection and Protection in Multics

- [Protection](10-13/protection/)
- [Protection and Information Sharing in Multics](10-13/multics/)

## Lecture 5 - UNIX and Plan 9

- [The UNIX Time-Sharing System](10-15/unix/)
- [Plan 9 From Bell Labs](10-15/plan9/)

## Lecture 6 - Medusa and Pilot

- [Medusa: An Experiment in Distributed Operating Systems Structure](10-20/medusa/)
- [Pilot: An Operating System for a Personal Computer](10-20/pilot/)

## Lecture 7 - Monitors and Mesa

- [Monitors: An OS Structuring Concept](10-22/monitors/)
- [Experience with Processes and Monitors in Mesa](10-22/mesa/)

## Lecture 8 - Monitors and Mesa

- [The Distributed V Kernel](10-27/distv/)
- [Sprite](10-27/sprite/)

## Lecture 9 - Grapevine and Global Memory Management

- [Monitors: An OS Structuring Concept](10-29/grapevine/)
- [Experience with Processes and Monitors in Mesa](10-29/gmm/)

## Lecture 10 - Microkernels and Exokernels

- [The performance of Micro-Kernel-Based Systems](11-3/ukernel/)
- [Exokernel: An OS Architecture for Application-Level Resource Management](11-3/exokernel/)

## Lecture 11 - VM/370 and Xen

- [The Origin of the VM/370 Time-Sharing System](11-5/vm370/)
- [Xen and the Art of Virtualization](11-5/xen/)

## Lecture 12 - Mach and Vax VMMS

- [Machine-Independent Virtual Memory Management for Paged Uniprocessor and Multiprocessor Architectures](11-10/mach-vmm/)
- [Virtual Memory Management in the Vax/VMS Operating System](11-10/vax-vmm/)

## Lecture 13 - Unix Filesystems

- [A Fast File System for Unix](11-12/fs-unix/)
- [The Design and Implementation of a Log-Structured File System](11-12/lsfs/)

## Lecture 14 - Rio and Soft Updates

- [The Rio File Cache: Surviving OS Crashes](11-17/rio/)
- [Soft Updates: A Solution to the Metadata Update Problem in File Systems](11-17/soft-update/)

## Lecture 15 - Lottery Scheduling and Scheduler Activations

- [Lottery Scheduling: Flexible Proportional-Share Resource Management](11-19/lotto/)
- [Scheduler Activations: Effective Kernel Support for the User-level Management of Parallelism](11-19/sched-activ/)

## Lecture 16 - Cells and TaintDroid

- [Cells: A Virtual Mobile Smartphone Architecture](11-24/cells/)
- [TaintDroid: An Information-Flow Tracking System for Realtime Privacy Monitoring on Smartphones](11-24/taintdroid/)

## Lecture 17 - GFS and BigTable

- [The Google File System](12-1/gfs/)
- [Bigtable: A Distributed Storage System for Structured Data](12-1/bigtable/)

## Lecture 18 - MapReduce and Haystack

- [MapReduce: Simplified Data Processing on Large Clusters](12-3/mapreduce/)
- [Finding a needle in Haystack: Facebook's Photo Storage](12-3/mapreduce/)

## Lecture 19 - Tensorflow

- [TensorFlow: Large-Scale Machine Learning on Heterogeneous Distributed Systems](12-8/tensorflow/)