---
layout: page
title: CSE 221 - Operating Systems - Notes on "Bigtable; A Distributed Storage System for Structured Data"
permalink: /education/grad/cse221/12-1/bigtable/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, OS, google, file, system, distributed, bigtable, big, structured
description: Notes for "Bigtable; A Distributed Storage System for Structured Data"
mathjax: true
---


## Intro

- massively parallel database
- data model
    - sparse, distributed persistent, multi-dimensional sorted map
    - map indexed by row key, column key, and timestamp
    - value is uninterpreted array of bytes
    - `(row:string, col:string, time:int64)`
- row key up to 64KiB in size (usually 10-100 bytes)
- all r/w atomic
- ordering lexicographically by row key
- row ranges stored in tablets (unit of load balancing)
- column keys are grouped into column families
    - basic unit of access control
    - data stored in a column family typically of same type
    - column family must be created before data can be stored in a col key
- timestamps for versioning
- API allows creating tables/deleting tables and column families
- bigtable stores data in GFS
- depends on cluster management system for scheduling jobs/managing resources
- SSTable file format used internally to store bigtable data
    - sstable is persistent ordered immutable map from keys to values
    - operations provided to look up sstable with key to retrieve value
- bigtable relies on distributed lock service called chubby
- components
    - client library
    - master
    - tabelet servers
- master responsible for
    - assigning tablets to talet servers
    - detecting addition or expiration of tablet servers
    - balancing tablet load
    - garbage collection
    - schema changes
- tablet server responsible for managing tablets and
- three level hierarchy to determine tablet location
    - root tablet
    - mid-level metadata tablet
    - table tablets
- client lib caches tablet locations
- tablet assignment
    - tablet assigned to one tablet server at a time
    - master keeps track of set of live tablet servers and which tablets
      are on each
    - chubby for tablet discovery
    - tablet server stops serving if it ever loses the exclusive lock on
      its tablets
- master startup sequence
    - master grabs unique master lock
    - master scans servers directory in chubby to find live tablet servers
    - master communicates with every live tablet server to discover
      their tablets
    - master scans METADATA table to learn about the set of tablets.
- tablets stored persistently in GFS
    - updates committed to log of change records
    - buffered by a memtable
- read/write permissions checked by tablet server upon connection
- compactions
    - write operations can increases memtable size
        - when hitting a threshold, write out memtable buffer to SST file
        - minor compaction
    - merging SSTables by a process in background
    - merging compaction rewrites many SSTs into a single SST
- clients can group multiple column families into locality groups
    - separate SST is generated per locality group
        - tablet is assigned per SST, so grouping into a single tablet
          can improve locality for column access
- client can control compression and format of SST
- cache key-value pairs in scan cache
- bloom filters to quickly determine if a key exists
- commit log for many tablets into single commit log per tablet server
- moving tablets forces a compaction on the tablet - helps reduce
  recovery time for master
- other lessons
    - memory corruption
    - network corruption
    - clock skews
    - hung machines
    - extended, asymmetric net partition
    - bugs in other systems (e.g. chubby, GFS)
    - planned/unplanned HW maintenance
    - delay adding features until knowing how they will be used

## Lecture notes

- database on GFS for google services
- distributed multi-level map
- FT, persistent
- scalable
    - thousands of servers, TBs of memory, petabytes of data
- self-managing
- bigtable started NoSQL trend
    - new way of thinking about database technologies
    - NoSQL is non-relational for analysis of high volume, disparate data types
- why NoSQL?
    - flexible schema-less data model
    - dynamic schemas
    - auto-sharding
    - replication
    - integrated caching
- 