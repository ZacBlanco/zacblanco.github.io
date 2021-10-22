---
title: Clio; A Hardware-Software Disaggregated Memory System
type: page
conf: arxiv
year: 2021
link: https://arxiv.org/pdf/2108.03492.pdf
---

- **MN**: Memory Node
- **CN**: Compute Node

3 questions:

- how does the design and implementation of dedicated hardware MN differ from
  the server and prorammable NIC designs
- How can a low-cost MN host TBs of memory and support thousands of concurrent
  applications?
- How to minimize tail latency?

novel approach for MN: cycle-bounded PT lookup -- overflow-free hash-based page
table lookups (single DRAM access)

- Clib overview
  - separate remote address space (RAS) different from the CN VAS.
    - allocate remote memory via CLIB --> ralloc, rfree, rread, rwrite
  - operations can be `SYNC` or `ASYNC`
  - intra-thread ordering: synchronous calls are strictly ordered. Asynchronous
    APIs are release ordered.
  - threads/processes can share memory when not in the same CN
-

MN CBoard Design

- fast address translation
  - used fixed metadata size for PT lookups (0.4% of page table size @ 4MB
    pages)
  - value of VA and PID used as index to determine hash bucket of a PTE
  - prevent hash collisions to at VA allocation time
    - check at allocation time if it results in an overflow, otherwise generate
      a new VA range
      - tradeoff for latency at allocation time vs access time
  - implements a TLB and uses CAM on the fast path
  - other downside is that fixed VAs (e.g. `mmap(MAP_FIXED)`) might not work if
    the fixed VA is not available
- low-tail latency page fault handling
  - oversubscription on memory node
  - example:
    - application uses CLib to `ralloc`
    - cboard handles request on slow path with ARM cores that
      - allocates VA address range by finding a set of slots in the hash page table
      - forwards PTEs to the fast path to insert into the page table
      - PTEs are marked as invalid
    - VA address return to CLib client
    - client performs a write, request goes to fast path
    - TLB miss causes a PTE fetch
    - Invalid PTE entry page fault handler triggered (on cboard) that fetches a free physical address from an async buffer and esablishes the valid PTE.
    - write executed, PTE updated, and inserted into TLB
    - return to user/finish request
- stateless/bufferless networking for MNs
  - MNs need to be low-cost and simple
    - high performance networking is expensive
    - MNs strip all network and only operate at ethernet layer with small code
      for ack generation
    - most logic sits in the clib which handles the transport and retry at the
      request-level
