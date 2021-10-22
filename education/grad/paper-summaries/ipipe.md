---
title: Offloading Distributed Applications onto SmartNICs using iPipe
type: page
conf: sigcomm
year: 2019
link: https://homes.cs.washington.edu/~arvind/papers/ipipe.pdf
---


## Intro

- How to efficiently maximize offloading benefits for distributed applications?
- smartNICs comprised of NIC hw, small CPU, onboard DRAM, packet processing,
  domain specific accelerators, programmable DMA engine
- characterize current smartNIC performance, using results to motivate the
  design of iPipe to efficiently utilize resources

## Characterizing SoC SmartNICs

- on-path and off-path smartNIC design
  - on-path --> cpu cores part of the processing pipeline, cannot be bypassed
  - off-path, CPU cores capture data coming from a NIC switch which chooses to
    send packets to NIC cores or host cores (via DMA)
- traffic control
  - packet transmission through a smartNIC core incurs nontrivial cost
  - using more cores, doesn't scale perfectly. High overhead cost.
  - Difficult to reach line rate using NIC cores and small packets
  - on-path NIC cores can more efficiently pull work from a queue than off-path
    ones
- computing units
  - execution time of tasks vary significantly from 1-2us up to 34/71us
  - wimpy processor best for running apps with low IPC or high MPKI
    (misses-per-kilo insn)
- onboard memory
  - five types: L1, packet buffer (SRAM), L2, NIC-local DRAM
  - on-path smartNICs favor inline packet manipulation
  - using more memory than available on smartNIC may result in worse performance
    than communication with the host
- host communication
  - blocking/non-blocking DMA
  - significant performance ebenfits using non-blocking DMA and aggregating
    transfers (DMA sctter/gather)


## The iPipe Framework

- goals:
  - programmability
  - computation efficiency
  - isolation
    - need to guarantee applications can't see each other

- iPipe uses an actor programming model for application development
  - easy to map whole actors onto a single physical computing unit
  - well-defined states/memory
- iPipe runtime provides:
  - actor shceduler with a hybrid FCFS/DRR scheduling algorithm
  - distributed object abstraction to support migration
  - security isolation mechanism

### actor scheduler

- schedule on both NIC and host CPU
  - need to ensure resources are being used efficiently
  - centralized queues on the NIC, decentralized on the host
  - FCFS shown to perform poorly when processing time has high variance (3)
  - otherwise, processor sharing is good for high-variance distrib.

- design:
  - some dedicated NIC FCFS cores, some dedicated DRR cores
  - upgrade/downgrade to/from FCFS or DRR depending on current tail latencies

- all cores start in FCFS mode, when an actor is pushed in the DRR queue,
  scheduler spawns a core for DRR execution
  - when all cores in DRR group in used, and FCFS has free cores, migrate a core
    to the DRR group
    - similar condition to move DRR core to FCFS group
  - only allow smartNIC to initiate migration because it is more sensisitive
    than the host processor when overloaded
- when migrating, shut down the queue and buffer requests until the actor is
  moved to the host

### statistics

- store mean (mu) of processing times
- measure requests latency (sigma)
- measure tail as mu + 3*sigma

### memory

- memory accesses via distributed memory objects (DMOs)
- actors are allocated max amount of memory when created
- memory accesses need to look up DMO addresses to read values
