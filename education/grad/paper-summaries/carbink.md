---
title: Carbink; Fault-Tolerant Far Memory
type: page
conf: osdi
year: 2022
link: https://www.usenix.org/system/files/osdi22-zhou-yang.pdf
---

### Abstract

- fault tolerance is critical for far memory
    - likelihood of failure increases as you scale to more machines
- techniques:
    - erasure-coding
    - memory compaction
    - one-sided RMA
    - offloaded parity calculations
- results:
    - 29% lower tail latency
    - 48% higher application performance (??? what does this mean)
    - maximum 35% addt'l memory usage


- carbink: far memory for efficient, high-performance fault recovery

- when evicting data from local RAM, writes erasure-coded versions of the data
to remote memory nodes.
    - equivalent redundancy to pure replication
- _span_ swapping granularity (multiple memory pages)
    - each span lives only in one location: local node or far memory

### design

- centralized memory manager
    - track liveness of compute/mem nodes (CN/MN)
    - coordinate assignment of regions to nodes

- registration+allocation
    - memory node _(de)registers_ allocatable memory w/ compute node
    - compute node asks for (de)allocation from manager

- span is contiguous run of pages
- spanset contains 1+ spans
- allocation requests rounded up to span size
- align each span to page size
- swaps far memory INTO local mem at span granularity
- local memory INTO far mem at spanset granularity
- no shared memory b/t applications

- allocate spans locally with the _local page heap_
    - array of free lists tracking 8KiB aligned free spans of varying sizes
    - each thread thread has private thread-local cache to reduce locking
      contention for allocations

- carbink object shuffling
    - move objects in local spans to other local spans
    - goals:
        - create hot/cold spans
        - garbage collect dead objects
    - avoid swap-in amplification
        - net transfer of add'tl data of entire span when only small object
          within the span is required

- fault tolerance
    - fig 4c
    - when swapping in spans, recompute parity from "matching" spansets (pairs
    which) fill in each other's missing spans
    - unplanned failures:
        - detection via timeouts/leases
        - on detection, spawn background threads to begin helping reconstruct
          affected spans
        - manager tells new MN which spans/parity needs to be read and what to
          recompute
        -
