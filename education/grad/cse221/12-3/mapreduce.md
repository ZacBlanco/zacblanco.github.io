---
layout: page
title: CSE 221 - Operating Systems - Notes on "MapReduce; Simplified Data Processing on Large Clusters"
permalink: /education/grad/cse221/12-3/mapreduce/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, OS, google, map, reduce, hadoop, research, processing, framework
description: Notes for "MapReduce; Simplified Data Processing on Large Clusters"
mathjax: true
---


## Introduction

- most computations are mundane
    - difficulty is in the distribution of work
    - hide details of parallelization, FT, data distribution, and load balancing
- inspired by map/reduce primitives in Lisp and other functional languages

## Programming Model

- input: set of kv pairs
- output: set of kv pairs
- express computations as Map and Reduce
- Map takes input pairs and produces a set of intermediate KV pairs
- library groups values with same intermediate key, and then passes them
  to a reduce function
- reduce, accepts an intermediate key and the set of values for that
  key, then merges them together into a single value.
- uses iterators when input set too large
- in addition to code user needs
    - write a specification object
        - names of input/output files
        - tuning parameters
- map: `(k1, v1) --> list(k2, v2)`
- reduce: `(k, list(v2)) --> list(v2)`
- original impl uses strings - up to mapreduce program to parse
- impl designed for switched ethernet
    - dual processor w/ 2-4GiB mem
    - commodity network hardware (100MBit/s or 1gbit/s)
- users submit jobs to a scheduling system. Job is a set of tasks and
  mapped by scheduler to a set of available machines in a cluster.
- split across machines by partitioning input data. Each partition is a
  **split**
- reduces are distributed by partitioning the intermediate key space in
  R pieces with a partitioning function
- number of partitions  is specified by the user.
- typically split into 16-64MiB/piece
- master process picks idle works and assigns each worker a map or reduce task.
- worker assigned a map reads corresponding split, then parses KV pairs
  from input and passes to the map function
- periodically, buffered pairs output from map are written to local disk and
  partitioned into R regions by the partitioning function
- when reduce worker is notified from master about the locations of intermediate
  pairs, it sorts the intermediate pairs by key.
- The reduce worker iterates over the sorted intermediate pairs, and then uses
  the reduce function to calculate the output. The final output file is created
  afterwards
- after all map and reduce tasks are done, the master returns back to the user
  program.
- output available across all of the R reduce tasks.
- master data structures
    - master stores task states, worker machine identities, also passes
      intermediate file names between map and reduce tasks.
- master pings workers periodically
    - no response --> mark worker as failed
    - map tasks reset and created on new workers.
    - completed map tasks from a failed work also restarted on machine
      failure because local disks on failed machine are no longer
      accessible
    - all workers notified of re-execution
- master failure
    - master can write periodic checkpoints of master data structures.
    - if master dies, read checkpoint.
    - not implemented - just fail job if master dies.
- assume idempotency of map/reduce tasks --> output is deterministic
  given an input
- without atomic reads/writes of data, then mapreduce could have diff results
- use GFS locality to determine which workers should handle particular
  sets of files
- map input size is typically chosen based on GFS chunk size
- when close to completion, but still waiting on straggler, schedule
  "backup executions" on any other workers.
- refinements
    - automatically partition with hash(key) mod R
        - instead, allow users to define a hash function for partitions
    - ordering guarantees
        - guarantee within a partition the given values are processed in
          increasing order.
          - help generate sorted output files
    - combiner function
        - optional partial merge of data before being sent over the network
        - save bandwidth
- anything application developer does, such as writing auxiliary data,
  etc during map reduce functions are the responsibility of the
  programmer to make idempotent
- provide a mode of execution to skip records which cause failures due to bugs,
  etc
- local execution available to debug programs before sending to entire cluster.
- master runs HTTP server to expose status of tasks and entire job



## Lecture Notes

- programming API
- hide parallelism of outputs
- developer responsible for
    - input/output file spec
    - number of maps tasks
    - number of reduce tasks
    - W number of machines
    - write map and reduce functions
    - submit job
- _requires no knowledge of parallel/distributed systems_
- dev needs to find a way to split the FS
- intermediate files creates from _map_ tasks are written to local fisk
- output files are written to a DFS.
- optimize tasks with locality of GFS chunks
- one instance is the master which finds idle machines and attempts to assign the tasks.
- no reduce can begin until maps are complete
- if map worker fails at any time, task must be completely re-run
- mapreduce library does most of the hard work.
- periodic pings from master to determine if workers are up
- master writes periodic checkpoints in case master fails
- on errors, workers send "last gasp" UDP packet to master
    - detect records that cause deterministic crashes and skips them
- mapreduce great for one-pass computation
    - multi-pass...much slower
- no efficient primitives for data sharing
    - state between steps persisted to DFS
    - slow b/c of replication/storage
    - no control of partitioning across steps