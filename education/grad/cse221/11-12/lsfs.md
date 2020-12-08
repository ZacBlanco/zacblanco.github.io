---
layout: page
title: CSE 221 - Operating Systems - Notes on "The Design and Implementation of a Log-Structured File System"
permalink: /education/grad/cse221/11-12/lsfs/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, OS, fs, unix, log-structured, lsfs, file, filesystem
description: Notes for "The Design and Implementation of a Log-Structured File System"
mathjax: true
---


## Introduction

- published 1991
- log structured filesystem writes modifications to disk sequentially
    - speeds up writing and crash recovery.
    - only structure on disk. contains indexing info to read back information
- log divided into segments
    - add segment cleaner to compress live information from heavy fragmentation
- high performance (7-10x greater)
- no random-access structures
- must always have large extents of free space available to write new data
- solution based on "segments"

## Design of FS for 90's

- Technology
    - disk technology improve rapidly, more in cost and capacity than
      performance though.
    - main memory size increasing at an exponential rate
        - allows absorbing of disk load to be spread out over a period of time
- Workloads
    - common workloads: small files; hpc not the focus
    - problems with current FS
        - current FSes do synchronous writes and many seeks: destroys perf
    - perf dominated by metadata writes rather than data writes.

- Log Structured FS
    - log sequentially to disk, prevent seeks.
        - how to retrieve info from log?
        - how to manage free space? (difficult)
- File location and reading
    - still uses inode structure
        - 10 blocks direct in inode, rest in indirect block
        - inode map structure maintains current location of each inode. inode
          map divided into blocks, written to log.
        - inode maps typically kept in mem
- Free space management
    - need to keep large swaths of empty space open, prevent fragmentation
    - use a combination of threading and copying to compact log and free up
      segments.
    - bulk segment operations to utilize entire bw of disk.
- segment cleaning
    - moving live data out of a segment into smaller clean segments.
    - need to identify live blocks of segments.
    - need to identify file for which each block belongs, and position of block
      in file.
      - use segment summary block
      - no free block list or bitmaps
- four policy issues for segment cleaner
    - when to execute?
    - how many to clean at a time?
    - which to clean?
    - how should live blocks be grouped?
- first two policies not addressed in current work
    - _write_cost_ to compare cleaning policies.
        - avg amount of time disk is busy per byte of new data written, including all cleaning overheads
    - performance of log-structured FS improves if there is more disk space.
- require a bi-modal distribution with most segments nearly full, few
  empty/nearly empty and cleaner almost always works with empty segments.
  - allows high overall disk capacity with low write cost.
- hot/cold segments should be treated differently.
    - hot: constantly change, not worth reclaiming
    - cold: unlikely to be used again soon, reclaim if possible
- segment usage table
    - for each segment a data structure that records number of live bytes per
      segment, and most recent access time
    - segment summary records age of youngest block to segment
- checkpoint and roll-forward for recovery
- special log operation for each directory change. Includes opcode, location of
  entry, contents of entry (name+inode num) and new ref count

- implementation and analysis
    - distribution of segment utilization is far more extreme on modalities than
      initially conceived


## lecture notes

- problems with FFS
    - layout:
        - related files separate
        - inodes separate from files
        - dir entry separate from inode
        - only 5% utilization
    - sync writes
        - metadata requires synchronous writes
        - w/ small files, most writes are metadata
            - sync writes defeat caches