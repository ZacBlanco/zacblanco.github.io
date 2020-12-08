---
layout: page
title: CSE 221 - Operating Systems - Notes on "Soft Updates; A Solution to the Metadata Update Problem in File Systems"
permalink: /education/grad/cse221/11-17/soft-update/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, OS, fs, unix, soft, metadata, file, filesystem
description: Notes for "Soft Updates; A Solution to the Metadata Update Problem in File Systems"
mathjax: true
---


### Introduction

- published May 2000
- low cost sequencing of of updates to write-back cache blocks.
- cost of writes are reduced in two places by a write-back cache
    - multiple updates to a single metadata component
    - multiple updates to a single block of metadata
- soft updates is a technique to sequence fine-grained updates
    - track dependencies among blocks.


### Metadata Update Problem

- updates need to be propagated to disk in a particular order.
- e.g. adding a new file in a directory, there are two steps
    - write inode to disk
    - write directory entry
- if dentry is written and system crashes, FS is inconsistent - no idea
  about inode state.
- Rules
    - never point to a structure before it has been initialized
    - never re-use a resource before nullifying all previous pointers to it
    - never reset the last pointer to a live resource before a new
      pointer is set
- many implementations ignore update dependencies --> reduce integrity
- looking for solutions which don't change the on-disk structure
- async writes for metadata updates --> pass sequencing to disk scheduler?
    - cannot be used safely if sequencing is required.
- ideal solution provides immediate stability and consistency to all
  metadata updates with no restriction to on-disk data organization,
  little to no overhead, and no special hardware.
- ideal perf characteristics
    - applications should never wait for disk writes unless they
      explicitly choose to do so
    - system propagates modified metadata to disk using minimum possible
      number of disk writes, given the allowed window
    - minimize amount of main memory needed to cache dirty metadata
    - cache write-back code and disk request scheduler should not be
      constrained in choosing what blocks to write to disk beyond bare
      minimum to guarantee consistency

### Soft Updates

- maintain dependency information associated with dirty in-memory copies of
  metadata to keep track of sequencing requirements.
- initially envisioned a DAG - implementation of maintaining DAG was difficult.
- cyclic dependencies eventually appeared because of multiple inodes or
  directory metadata in a single block.
- fs recovery can be done immediately
    - no need to run fsck before mount
        - but can still have dangling pointers:
            - unused blocks not in free map
            - unreferenced inode not in free inode map
            - inode link count exceed actual number of links
        - run fsck to fix
- when fs returns to caller, no guarantee the write is permanent
- soft updates means loss of up to 30s of information

### Implementation

- fsync causes slowdowns and requires pruning dependencies
- unmount requires finding and flushing dirty blocks
    - more activity than anticipated
- syncer daemon cause of useless writebacks
    - fix syncer daemon to remove 50% of useless writes.
- large directory modifications can result in hundreds of inodes in lost+found


- Performance generally better than the previous FS in all areas


## Lecture Notes

- need to order operations so that metadata is consistent in the case of a crash
  - create a file: cannot update directory block on disk before the new
    inode is safely written to disk
  - delete a file:
- rule of thumb when considering dependency updates
  - OK to waste inode or data blocks on disk with nothing pointing to it
  - cannot have directory or inode or index block pointing to some garbage
- from user point of view
  - avoid confusing users
- conventional FS
  - metadata updates done through synchronous blocking writes
  - increases cost of metadata updates
- soft updates
  - write-back caching of metadata updates
  - why?
    - performance: eliminate sync overhead
    - integrity: handle all potential corruption
    - recovery: instant mounting
  - how does it solve the problem?
    - update metadata in memory in place (apps see latest version)
    - track updates to unwind/roll forward dependency tree
    - when writing to disk, unwind dependencies
    - when done, roll forward updates
    - lock during writes
    - granularity of updates is inodes
  - roll back metadata in one memory block to an earlier, safe state in memory
  - solution
    - write first block with metadata that is rolled back (e.g. A')
    - write blocks that can be written after first block has been written (Block B)
    - roll forward a block that was rolled back
    - write that block
    - breaks cyclical dependency, but must now be written twice
- with soft updates, you can free a large file, but disk space won't be updated
  for 30 seconds o more
- if soft updates are within a small percent of no update ordering, does LFS
  have any chance to improve upon it?
