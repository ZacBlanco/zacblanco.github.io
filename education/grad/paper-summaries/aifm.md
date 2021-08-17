---
title: AIFM; High-Performance, Application-Integrated Far Memory
type: page
conf: osdi
year: 2020
link: https://www.usenix.org/system/files/osdi20-ruan.pdf
---


## Notes

- ram is constrained, may be available elsewhere, yadda, yadda, yadda
- avg memory utilization in DCs is 60%, CPU is 40%
  - 30% of server memory is "cold" and could potentially be reclaimed? [41]

> Moreover, the Linux kernel spins while waiting for
data from swap to avoid the overheads of context switch and
interrupt handling.
  - look for more info on this quote

- swaps pointers at runtime when encountering memory pressure.
- Key is "fast context switching" between green threads of a high-level runtime
  - allows more useful work to be done while waiting for data?
    - what kind of "useful work"?

- key ideas
  - remotable pointer
  - pause-less memory evacuator
  - runtime APIs that "convey semantic information to the runtime"
  - remote device interface to offload light computations to remote memory

> The pauseless memory evacuator en- sures that application threads never
experience latency spikes due to swapping.

- ^^ is this basically just pinning the memory?

- other problem: IO amplification for data structures
  - AIFM operates at the object granularity (rather than page)
- comparison: GC is focused on removing dead/unreferenced objects
  - AFIM wants to evacuate/swap out old objects

pointer representation
  - `| hot 63:54 | present | shared | dirty 53:48 | evacuating | data 46:0 |`
  - need to read about c++ `std::unique_ptr` and `std::shared_ptr`

- challenge: managing lifetime of dereferenced data --> use scopes/lifetimes
  (e.g. rust)
- evacuation handlers
  - when an object is not protected by a deref scope, it may be evacuated
  - see hash table example in paper

>  When an object is remoted, any embedded
remoteable pointers must either be moved to the local heap, or
the object it references must be moved to remote memory, and
the remoteable pointer must be updated with an identifier to
later retrieve the remote object from a remote device (§4.2.4)

###  key ideas

  - remotable pointer
  - pause-less memory evacuator
  - runtime APIs that "convey semantic information to the runtime"
  - remote device interface to offload light computations to remote memoryimplementation

> AIFM’s runtime is built on “green” threads (light-weight,
user-level threading), a kernel-bypass TCP/IP networking
stack, and a pauseless memory evacuator

...yeah ok kernel bypass just seems like "cheating" to me. I get it's
high performance but were other systems benchmarked with ? What is the
performance without bypass?

- prevent latency by switching green threads' work to prevent kernel ctx-switch



