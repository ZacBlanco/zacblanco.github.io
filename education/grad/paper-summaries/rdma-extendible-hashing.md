---
title: One-sided RDMA-Conscious Extendible Hashing for Disaggregated Memory
type: page
conf: ATC
year: 2021
link: https://www.usenix.org/system/files/atc21-zuo.pdf
---


## Introduction

- focus on disaggregated memory setting
- distributed hashing indexes used in many applications
- RDMA reads are executed by client CPUs
- insert-update-delete (IDU) handled by remote CPUs
  - in disaggregated memory, not enough CPU on remote side to handle IDUs
  - RDMA client can achieve the IDU, using write and atomic verbs, but incurs
    significant performance penalties
- challenges with current impl
  - handling hash collisions for reads/writes
  - concurrency for remote access
  - remote reizing of tables
- contributions:
  - one-sided RDMA-conscious table structure
  - lock-free remote concurrency control
  - extendible remote resizing

## Motivation/background

- Other hashing schemes require multiple operations to resize
- cuckoo hashing scheme has to do many read/writes to handle evictions of keys
  to other locations in the table -- not good for disagg memory
- FaRM hopscotch hashing requires lots of ops to find an empty slot in the
  neighborhood of the hash key -- this also incurs many read/write ops
- DrTM hashing requires traversing buckets one by one until finding an empty
  slot. Insertions also require locking on buckets which degrades performance
  significantly


## RACE hashing

- to fix resizing performance, use extendible hashing with a directory where
  clients can cache the directory
  - don't need to copy all items over w/ directory, but requires extra read.

### RAC subtable

- associative structure
  - each bucket has multuple slots capable of storing multiple KV-items
  - K-way associativity means each bucket has K slots
- two choices
  - two hash functions compute two separate locations
- overflow colocation
  - overflow bucket location combined with main bucket (less reads potentially)

- claimed advantages
  - RDMA IDU friendly
    - each key only involves two combined buckets
    - request ops within buckets without evicting/moving items to/from buckets
  - RDMA search friendly
    - search issues 2 RDMA reads
    - retrieves only combined bucket
    - two RDMA reads in parallel to reduce latency
  - high memory efficiency
    - assoiativity and two choices, subtable is more memory efficient

### lock-free concurrency control

- RDMA CAS locks are expensive (at least one RT)
- RACE can achieve lock-free except for failed insertions
- 8 bit fingerprint w/ 8 bit KV length
- because 8 bits used for length, KV pair can max be 16KB (each unit is 64B)

- lock-free insertion
  - read two combined buckets that a key corresponded
  - at the same time, client writes the KV block into the memory pool
  - after receiving combined buckets, cient finds empty slot in (1) main bucket
    first and (2) in the overflow bucket
  - if empty slot found, use CAS to write pointer of the KV block
  - re-read combined bucket to check for duplicate keys
- lock-free delete
  - read value, set to null if found using CAS
- lock-free update
  - search target key, and write key to memory pool at the same time
  - once known target key exists, write with an RDMA CAS
- lock-free search
  - search key by reading the combined buckets
  - if fingerprint matches a slot, client reads KV block of the slot

### Client directory cache w/ stale reads

- reduce RDMA read for accessing directory by caching it at the client
  - potential for stale reads
- basic solutions:
  - client broadcasts a cache invalidation message on directory update
  - pilaf proposed to close RDMA connections of other clients from performing RDMA
    reads once resizing is triggered (all other clients must reconnect)
- RACE solution:
  - client can verify local depth and directory suffix bits match hash
  - if they mismatch, then the table must have been resized
  - local depth mismatch means subtable was resized
  - both mismatch, the client can verify which subtable resized
- requires not changing initial directory ptr


## eval

- fig 10/11/12 -- what is the KV size?
