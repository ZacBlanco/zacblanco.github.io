---
title: TPP; Transparent Page Placement for CXL-Enabled Tiered Memory
type: page
conf: arXiv
year: 2022
link: https://arxiv.org/pdf/2206.02878.pdf
---


- authors from uMich and FB
    - FB exploring CXL?

- argument: observe a wide adoption of tiered memory systems
    - is this any different than today?

- this paper: characterize memory usage of DC apps across FB
    - claims: opportunities to utilize tiers, but if not done correctly,
      performance will suffer
    - system: OS page placement policy for different memory tiers
        - page allocation separate from reclamation
        - promotion/demotion of pages across tiers

### Intro

claim:

> For each application, we want to know how much of its memory remains hot,
> warm, and cold within a cer- tain period and what fraction of its memory is
> short- vs. long-lived.

Are these the only things? Are these the most important?

- existing IPT (Idle page tracking) do not work due to requiring kernel
  modifications
    - not sure if they're going for measurements why they couldn't do this
    - I guess production is serious..can't let researchers muck with it

- Built "Chameleon" -- userspace tool using CPU precise event sampling to
characterize application mem access and behavior
    - track LLC/TLB misses
    - generate heatmaps of memory access

main draws:
    1. workloads have meaningful warm/cold memory
    2. large fraction of anonymous memory is hotter, file-back memory is colder
    3. page access patterns are stable for meaningful durations
    4. workloads have varying sensitivity

- no citations on CXL latency
    - claim is 50-100ns additional access latency?
- 55-80% of applications allocated memory remains idle (s3.3)


### Memory Usage of DC Applications

- still don't understand why the kernel modifications aren't possible...
- use PEBS of the CPU PMU to dump events
    - divide cores into groups, and enables sampling per group
    - hand-wavy 1/200 samples per PMU event
- virtual-physical page address translation (what's the benefit?)
    - can be disabled due to overhead for large processes
- maintain a bitmap to track activeness within an interval
    - if page active within interval, bit is set.

#### Production Workloads

- Web -- VM with JIT runtime to serve web requests
    - thread assigned request until completion
- Cache -- large distributed memory object caching service between web and
  database
- data warehouse
    - unified computing engine for parallel data processing
    - complex batch queries, TBs of memory/days to complete query

- author observations
    - file-backed mem less hot than anon pages
    - for web/warehouse workloads, most mem is cold


- cache2 workload, why does anon increase/file-backed decrease over time?
    - what happens to this trend in the long run?
- web app has GC spikes. What does this means for memory accesses?
- hard to understand fig 10. What is throughput %? Throughput of what?

#### Background on Linux Page Placement

- kernel assumes homogeneous DRAM-only NUMA nodes

- kernel always attempts to allocate node-local pages
    - 3 watermarks: min, low, high
    - if free pages goes below _low_, node under mem pressure and initiates
      reclamation
    - reclamation must bring it back to the _high_ watermark in order to
    continue allocating pages on that node


### TPP

- Features:
    - lightweight demotion to CXL-Memory
        - instead of reclaiming pages, migrate to CXL-memory instead, reclaim
          later
        -
    - decoupling allocation+reclamation
        - demotion watermark
            - pages demoted until demotion watermark hit
        - allocation watermark
            - new pages cannot be allocated after the allocation watermark
    - hot-page promotion to local nodes

### Evaluation

- HW: FPGA-based CXL memory card

> We have confirmation from two major x86 CPU vendors that the access latency to
> CXL-memory is similar as the remote latency on a dual-socket system.
