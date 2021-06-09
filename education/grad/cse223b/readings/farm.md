---
layout: page
---

## Introduction

- memory system to exploit RDMA
- expose memory of many machines as shared address space
- transactions to allocate/read/write/free
- fast message passing primitive
- local memory is still 23x faster

## FaRM

- one-sided rdma reads to access data
- rdma writes for the message passing primitive
  - circular buffer to implement unidirectional channels
  - sender writes messages to buffer tail, and advanced the tail pointer
  - never write past copy of HEAD
- TCP latency ceiling much higher
- issues with RDMA: significant perf hit as more memory registered
  - PhyCo kernel driver to allocate physically contiguous+aligned memory regions
- share queue pairs per machine to reduce number of queues on the NIC

### API etc

- all operations must alloc a new transaction
- `tx[Alloc|Free|Read|Write|Commit]`
- 2GB pages are the unit of recovery, registration, etc
- consistent hashing to map region identifier to machine storing the object
- optimisitic concurrency control w/ 2PC
  - tx ctx record version numbers
  - machine running txn is coordinator
    - send prepare to all participant (primaries and replicas of object in write set)
    - primary locks modified objects
    - all primary+replica logs message before sending reply
    - coordinator sends **validate** message to primaries in read set to
      check if the latest version was read
    -
