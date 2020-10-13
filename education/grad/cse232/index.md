---
layout: page
title: CSE 232 - Databases Systems
permalink: /education/grad/cse232/
keywords: CSE, 232, UCSD, UC, San Diego, software, operating, system, database, dbms, rocksdb linux, C, rust, yannis
description: Graduate Database Systems for CSE 232 at UC San Diego
mathjax: true
---


## Lecture 1

- [SQL Review](review/)

## Lecture 2 - Database Storage and Hardware Design

- Different storage types used by databases
    - Typically cost is higher and amount of storage is lower the faster it is
    - CPU-level cache typically isn't controlled by the software, so the DB can't do much optimization around it
    - RAM is utilized by DB, but can't guarantee transactions due to volatility
    - SSD, HDD, Tape, etc are non-volatile and can guarantee transactions


Peculiarities of storage and algorithms

- most storage is accessible via block-based access (via kernel, i.e. block device)
- not just pulling the bytes we need - taking big chunks.
- clustering (for hard disks):
    - cost is lower for consecutive blocks.
- sequential read times have increased immensely
    - minimum access time to first byte has not decreased propotionally

Ex: 2-phase merge sort

- Pass 1: Read section by section into buffer, sort all data, write
  sorted section to file
- 2nd pass: file pointers to all previously written files, read block from each
  file write in sorted order out to new file.
- Assume RAM buffer of size $$M$$
- Block size of $$B$
- Number of block reads = $$M/B$$
- File sections: $$N / M / B$$

With ram buffer of size $$M$$ can sort up to $$M^2 / B$$ records (at least one
block from each ram buffer)

Horizontal Placement of SQL data in blocks

- row oriented - tuples packed together in blocks
    - improve total table scan time
- for deletions, clear the block, add a "mark" (bit?) for deletion
- for additions to a sorted table by a key, leave a pointer on the block where
  it should exist to the new record (only applies on sorted files)
