---
title: Can Far Memory Improve Job Throughput?
type: page
conf: eurosys
year: 2020
link: https://people.csail.mit.edu/aousterh/papers/cfm_eurosys20.pdf
---

## intro

- availability of memory is limiting applications in compute clusters
- far memory does not solve fundamental problems
  - overcomes the "capacity wall" issue [48]

- two barriers:
  - swapping mechanisms need to be improved
    - traditional RDMA suffers from HoL blocking
    - interrupt handling/page reclamation on critical path
  - how to split and schedule memory demands
    - far-memory aware scheduler
    - unclear about optimal algorithms
      - design for max concurrency or speed?

## context

- memory configurations for achieving maximum bandwidth constrain granularity
of memory upgrades, otherwise suffer performance degredation
- deployment
  - don't focus on bottom-up design, rather upgrade paths for existing clusters

# CFM

- enable clusters to improve E2E throughput by leveraging far memory on
  dedicated memory servers
  - end-to-end makespan, or the time it takes to finish executing a list of
    jobs
- mostly transparent, or via explicit APIs
- swapping doesn't _have_ to be slow
- one-sided RDMA read/write operations
- fast swapping
  - OS typically prefetches several pages on pgfault, required page not always
    in the set and can be blocked.
  - existing RDMA swapping systems rely on interrupts which can add 10us or more
    latency
  - once faulted page are in local memory the OS charges the cgroup. If
    reclamation is required it happens before returning to the process, delaying
    processing.
- scheduling
  - current schedulers don't take into account possible far-memory use.

## fastswap implementation

- queue pairs per-CPU. critical operation queues are polled, non-critical use
  interrupts
- frontswap interface changed to distinguish critical+non-critical operations
- page fault handler prefetches pages and overlaps alloations with reads(?)
  - need to ask question about this
- offload reclamation to dedicated thread which allows processing to occur
  asynchronously
    - this means processes can go over cgroup allocation if reclamation takes
      too long.
    - allow buffer for every cgroup (~8MB) to perform async reclamation before
      falling back to direct reclamation.
- different applications degrade in different ways w/ far memory
  - use profiles to optimize scheduling to reduce degredation for jobs which
    tolerate far memory better
