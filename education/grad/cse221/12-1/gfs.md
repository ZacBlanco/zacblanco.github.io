---
layout: page
title: CSE 221 - Operating Systems - Notes on "The Google File System"
permalink: /education/grad/cse221/12-1/gfs/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, OS, google, file, system, distributed
description: Notes for "The Google File System"
mathjax: true
---

## Introduction

- GFS
    - distributed file system
    - commodity hardware
    - fault tolerance
- made for huge files (Multi GB)
- not designed for random reads - mostly for sequential workloads

## Design Overview

- assumptions
    - Assume system is built off of inexpensive and commodity machines
    - stores a modest number of large files
    - workloads are large streaming reads and small random reads
    - workload have many large sequential writes and appends
    - FS must be efficiently implemented and have well-defined semantics
    - high sustained bandwidth more important than low latency
- Uses familiar interface of create, delete, open, close, read, write
    - snapshot mechanism
    - record append
- arch
    - master w/ multiple chunkservers
    - files are divided into fixed-size chunks
    - chunk has 64-bit chunk id (unique)
    - master maintains FS metadata, ACLs, etc
    - GFS client code linked to each app implements the API for the cluster
- single master vastly simplifies design
    - utilization needs to be minimized to handle all r/w
- chunk size set to 64MiB
    - large chunk reduces metadata size
    - keep connection live for extended period of time to reduce
      communication latency
- chunk servers may be overloaded if file is not distributed properly.
- master has 3 types of metadata
    - file and chunk namespaces, mapping from files to chunks, and
      location of chunk replicas
    - all metadata kept in master memory
- namespaces and file-chunk mapping kept persistent by logging mutations
  to an operation log
- master gets chunk information from chunk servers upon registration
- in-mem data structures
    - keep data in memory so that metadata ops are fast
    - periodic scanning for GC
- master has limited capacity by master resources
- chunk locations determined via chunkserver heartbeats
- operation log of critical metadata changes


### consistency model

- relaxed consistency model
- FS namespace mutations are atomic
- state of a file region after data mutation depends on type of mutation
- region is consistent if all clients always see the same data
- region is defined after a mutation if it is consistent and client see
  the mutation writes in entirety.
- mutations are writes or record appends
- record append causes data to be appended atomically at least once
- use chunk version numbers to determine stale replicas
- identify failed chunkservers via handshakes


## System Interaction

- Leases and mutation
    - leases minimize management overhead at the master
        - initial time of 60s
        - chunk leased to clients
        - lease granted for a particular chunk replica
- order of ops
    - client asks master which chunkserver holds lease for chunk
    - master replies with identity of primary and secondary chunk replicas
    - client pushes data to all replicas
    - once all replicas acknowledge receiving data, client sends write
      request to the primary chunk
    - primary forwards write request to secondary replicas
    - secondaries reply to primary indicating a completed mutation
    - primary replies to the client
        - any errors reported
- if straddling chunk boundary, multiple write RPCs may be required
- atomic record appends
    - special mutation with small logic added to primary replica
    - client pushes data to all replicas of last chunk, then sends
      request to primary
    - if record append fails, client retries the operation
- snapshot
    - copies a file or directory tree simultaneously
    - COW techniques for snapshots
    - after leases revoked or expired, master logs snapshot operation to disk.
    - RPC from client triggers master to notify all chunkservers with
      files to create new chunks
        - no network transfer (disk 3x as fast as network)

## Master Operation

- namespace management and locking
- lock over regions of namespace for concurrent operation
    - no per-directory data structure to list files ina dir
    - no aliases or links
    - GFS represents data via prefix lists
    - read locks on all parents, write lock final directory
- r/w lock objects allocated and lazily deleted when not in use
- replica placement
    - rebalance chunks across racks
    - re-replicate chunks from failed chunkservers
- garbage collection
    - on deletion, space is not immediately reclaimed
    - master logs deletion, file renamed to hidden name w/ deletion timestamp
    - GC deletes file after coming across a deletion record in the log
    - gc easy in this system because master maintains all of the metadata


## Fault Tolerance

- HA
    - fast recovery
        - restore state in seconds
    - chunk replication
    - master replicated
    - op log and checkpoints on multiple machines
        - shadow masters to maintain log in case one master goe down
- data integrity
    - checksumming to detect corruption of stored data
    - chunks divided into 64KiB pieces
        - list of 64KiB checksum to detect particular place of corruption.

## Lecture Notes

- beginning of "big data" era
- challenges include:
    - capture
    - curation
    - storage
    - search
    - sharing
    - transfer
    - analysis
    - visualization
- data volume explosion
    - 44x increase in world data from 2009-2020
        - 0.8 zettabytes to 35ZB
- many different types of data storage
    - relational - e.g. DBs, transactions
    - text data (web pages)
    - semi-structured (XML/JSON)
    - streaming data (realtime, videos, etc)
    - big public datasets (weather, maps, etc)
- "Online Data Analytics" - Real time decisions via analytics (OLAP?)
- Slow decisions --> missed opportunities (finance)
- Data models
    - old model
        - few companies generate data, many consume
    - new model
        - many companies generate data, all use
- Hadoop project to mimic google software capabilities
    - GFS --> HDFS
    - Bigtable --> HBase
    - MapReduce --> MapReduce
- main motivations
    - failures are the norm
    - big files
    - appending writes
    - co-designing applications and file system API
- design criteria
    - detect, tolerate failures automatically
    - large files
    - long stream
    - sequential appending writes
    - concurrent appends with multiple clients
- master server holds all metadata (namespace, directory, ACLs, file-chunk mapping, chunk locations)
- master delegates consistency management
- GC orphaned chunks
- migrates chunks between chunk servers
- client record appends
    - large files as queues between producers and consumers
    - same control flow for writes except
        - client pushes data to replicas of last chunk of file
    - client retries append during failure
    - when data for append doesn't fit in chunk,, pad last chunk, and then create new chunk in file for the record.
- consistency model
    - changes to data are ordered as chosen by the primary replica
    - all replicas must be consistent
    - multiple writes from same client may be interleaved by concurrent operations from other clients
    - record append completes "at least once" at the offset of GFS's choosing.
        - may have duplicates
- leases and version numbers
    - if no outstanding lease when client requests write, grant new lease
    - chunks have version numbers
        - stored on disk at master and chunkservers
    - each time master grants a new lease, increment version and inform replicas
    - master can revoke leases
- 
