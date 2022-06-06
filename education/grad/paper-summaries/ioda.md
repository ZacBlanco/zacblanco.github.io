---
title: IODA; A Host/Device Co-Design for Strong Predictability Contract on Modern Flash Storage
type: page
conf: SOSP
year: 2021
link: https://dl.acm.org/doi/pdf/10.1145/3477132.3483573
---

## background

- SSDs have a lot of internal activities w/ non-dterministic characteristics
    - processes:
        - garbage collection
        - weat-leveling
        - buffer flush
- background IOs --> perf clashes w/ user apps
- GCs cause up to 60 percent latency spikes

- storage world has tried many new interface extensions to combat issue:
    - UNMAP/TRIM
    - ATOMIC_WRITE
    - STREAM
- most recent: NVMe: IOD (I/O Determinism)
    - one module of extension is the PLM (predictable latency mode):
        - two modes: deterministic and non-deterministic mode
- IOD delivers best latency in predictable mode, and only schedules background
tasks during unpredictable, or, "busy" mode
    - IOD expected to be useful on flash arrays
        - if one device is busy, the other devices can be in deterministic mode
        and read the parity bits to reconstruct the original data

- enhancements:
    1. predictability query
    2. piggybacking busy remaining time to assisnt host in picking less busy devices
     for reconstructions in the case of oncurrent internal operations
    3. strong latency window formulation and scheduling scheme

## how IOD-PLM works

- background ops only in busy windows when in "non-deterministic" mode
- two NVMe commands:
    - PLM-Query
        - host OS to query device state (IOs in future w/ deterministic latency)
    - GetPLMLogPage
        - toggle deterministic/busy state
- IOD-PLM is just "best-effort"
    - no strong contract
- opportunities for improvement
    - PLM-query has a lot of info, without guidance about how to use it
    - ND mode is too coarse-grained
        - SSDs have many parallel channels
        - GC on some channels may be ND or non-ND
        - device is declared busy as a whole
    - PLM busy window is vital for a strong guarantee, no work on formulating
    proper window size

- current approaches either introduce more questions or require app changes
- work builds on IOD-PLM


## IODA

- principles:
    - increase guarantee on predictability
    - limit device-side modifications
    - reducing host-SSD smeantic gap (SSDs array-aware)
    - fine-grained predictability
- PL_IO (predictable-latency flagged IOs)
- binary PLM query within the IO submission command
    - extend w/ a 2-bit PL flag (using 64-bit reserved slots)
        - tell the host an IO should be predictable
        - return to host if IO won't be predictable/contend with a GC
        - reconstruct block from RAID array using backup/parity SSDs
        - still susceptible to IO latency spikes with k > n unavailable SSDs
    - PL_BRT
        - extension to PL_IO
        - inform how long in response an IO might have to wait
        - might not work in practice due to GC "waves" where devices age at the same rate
- PL_win
    - PLM windows accepted by NVMe spec
    - device should alternate between two windows
    - every device must have enough over-provisioning space to absorb
      the largest-possible write bursts


