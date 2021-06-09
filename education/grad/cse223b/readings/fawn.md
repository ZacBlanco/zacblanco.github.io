---
layout: page
---

## Introduction

- efficient, massive parallel access to data
- _very_ energy efficient
- designed for IO-intensive workloads
- use a log-structured per-node datastore for efficient access to data
  - DRAM is expensive energy-wise compared to disk
  - append-only logs

- why fawn?
  - increases in CPU-I/O gap
  - CPU power consumption grows linearly with speed
  - dynamic power scaling is inefficient

## design

- fawn nodes join as physical nodes with a number of virtual IDs
- flash storage
  - fast access
  - slow random writes (1 byte as slow as 128KB-256KB)
- use only partial key fragment to perform lookups to use less dram for
  in-memory index
- semi-random writes to improve performance of maintenance on logs
- uses a front-end/backend architecture similar to the lab
 - frontend doesn't route requests, knows generally about all the nodes
 - consistent hashing to place put/get on particular nodes
- cache all the things!
- chain replication -- puts to head of chain, gets to the tail
-
