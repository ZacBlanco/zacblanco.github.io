---
title: Farview; Disaggregated Memory with Operator Off-loading for Database Engines
type: page
conf: arxiv
year: 2021
link: https://arxiv.org/pdf/2106.07102.pdf
---

## Introduction

- IO in databases has always been expensive
- optimizers push down selection and projection operators as fast as possible
- data movement, largest inefficiencies in computing
- use operators on disaggregated memory to reduce data transfer
- algorithm inefficiencies and network overhead are main drawbacks

## Farview System

- smart disaggregated memory, a buffer pool with extra bells and whistles
- executes an operator pipeline over the remote memory stream based on
  the client request
- supported ops: projection, selection, grouping, system support
  (encrypt/decrypt)
- FPGAs used to imeplement
- client makes request to farview
  - allocate dynamic memory region
  - net stack routes request to correct region
  - read request forwarded to memory stack, translates virtual address to
    physical on the FPGA
  - data streamed back to a dynamic region with the operator
- queues between memory and operator regions
- downsides:
  - dynamic region must be connected to the network, memory stacks have
    fixed locations (protection, performance)

- network implemented via one-sided RDMA read/write verbs
  - credit-based flow control, fair sharing
- memory stack
  - buffer pool using onboard DRAM on the FPGA
  - handles memory allocations, translations, and concurrent accesses
  - MMU has a TLB using on-chip Block RAM (fast FPGA memory)
  - 2MB aligned pages
- operator stack
  - operators attach to connection flows to/from memory
  - set of pre-defined dynamic regions
  - RDMA read/write params go directly to MMU, while query params go to the
    dynamic region

## Operators and Pipelines

- smart addressing
  - when a request only contains a small subset of columns from a table,
    performance benefits come if you only need to read the required columns
  - use specific requests to memory to prevent reading unnecessary data
- predicate selection, simply perform equality check against an argument
  provided in the request
- regex matching
  - included a regex matching library for FPGAs
- vectorization
  - use small vector size based on memory striping to execute on one or more
    data in the same cycle
- grouping/distinct operators
  - distinct: use an LRU cache for the hash table, pipeline
    - for collisions, send a buffer with collided values to buffer back
      to client
  - group by: all info from table before sending
  - aggregation: simple ops, count, min, max, sum, avg
-
