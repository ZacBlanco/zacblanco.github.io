---
layout: page
title: CSE 221 - Operating Systems - Notes on "A Fast File System for Unix"
permalink: /education/grad/cse221/11-12/fs-unix/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, OS, fs, unix, fast, filesystem
description: Notes for "A Fast File System for Unix"
mathjax: true
---





## Introduction

- published aug. 1984
- describes changes to the UNIX FS.
    - presents motivations, methods, and rationale, plus summary of results.
- need more bandwidth for new applications.

- "Old File System"
    - disk drive is divided into a number of partitions
    - each partition contains one FS
    - FS is described by a superblock containing basic parameters.
        - data blocks, max files, free list ptr, LL to all free blocks, etc
    - filesystem contains files. some files are directories which
      contain pointers to files that can also be directories (or normal files)
    - every file has an inode descriptor describing metadata about the file.
    - inodes reference first 8 blocks of a file directly.
        - double indirect blocks contain up to 128 ptrs to other data.
    - 150MiB filesystem consists of 4MiB of inodes, with 146MiB of data.
        - long seek from inode to data.
        - nonconsecutive block accesses
    - allocation of data blocks to files is suboptimal
        - never transfers more than 512 bytes per transaction
        - many seeks between 512 byte transfers.
    - fs performance more than doubled after doubling block size.
    - even with doubled block size, still only at 4% bandwidth of disk
    - free list scrambles causing out of order (seeks)
- new file system
    - replicate superblock for FT
    - minimum block sie of 4096 bytes to files at least 2^32 large.
    - disk partitions are split into one or more cylinder groups. cylinder group comprised of one or more consecutive cylinders on disk
    - bitmap of free blocks to replace free list.
    - static number of inodes per cylinder group
    - save cylinder group info one track further from beginning of
      cylinder in case of failures.
    - 4x data per txn
    - larger blocks leads to more wasted space
    - fragment some blocks to handle trailing parts of files.
    - wasted space with fragments is equivalent to fs with block size
      equal to fragment size
    - parameterized FS
        - driver handles HW, make other params configurable
        - split location of "rotationally optimal blocks" by placing
          info 8 equally spaced places apart.
        - rotational layout tables - each comp
    - layout policies
        - two levels: global decisions policies for new inode placement.
          Otherwise, local allocation routines to find locally optimal
          scheme to layout data.
        - improve perf by increase locality to minimize seek latency,
          and improve layout to make large transfers possible
        - need to spread across cylinder groups
        - directories have spearate allocation policy to spread
        - cylinders should not become full - hold data blocks of inodes
          on cylinder
            - redirect data block allocation on file > 48Kib
- performance
    - disk accesses for ls dir cut in half
    - disk access for ls dir with many files cut by 8
    - reads and writes measurably faster (5-10x)
        - read always at least write
        - write is more expensive since kernel needs to calculate for
          allocations
        - performance is limited by memcpy operations
        - chain kernel buffers to improve throughput
- functional enhancements
    - long file names (arbitrary length)
        - up to 255 chars?
    - file locking
        - implemented advisory locks
            - policy left to program on whether to obey or not.
            - locks applied or removed on open files.
            - process blocks if lock cannot be obtained (or can specify
              a try-lock type pattern)
            - no deadlock detection attempted
    - symbolic links
        - implemented as a file that contains a pathname
        - new syscalls for interacting with links
    - rename of a file
    - quotas
        - hard and soft limit prevents a user from utilizing the entire system

## lecture notes

- need to understand physical disk architecture.
- main point:
    - describe perf improvements
    - new features
- constraints on changes
    - maintain standard API
    - unix program compatibility
- problems in old FS
    - random data block placement for older FS
        - allocate closer together (cylinder groups)
    - inodes allocated far from data block
        - inodes and data close
    - small max file size
        - larger block size
    - waste from larger block size
        - use fragments
    - slow recovery
        - replicated superblocks
    - device oblivious
        - parameterize fs device characteristics
- two-level allocator
    - global:
        - localize related data within  a cylinder group
        - distribute unrelated data across cylinder groups
    - local:
        - optimal local placement within a cylinder group
            - first: rotationally optimal block
            - second: block within cylinder group
            - third: search cylinder groups
- new FS requires minimum free space to cylinder block allocations
- perf tightly coupled to free space on disk
    - reads always faster than writes, but in old FS writes could be 50%
      faster. why?
        - writes were async, so OS can re-order requests.
        - FFS block ordering and placement is important, so writing
          takes more time to allocate blocks
- What is now obsolete?
    - device parameters, host no longer knows info about disk hw
    - rotation allocation not done
        - blocks are allocated sequentially
        - drives do read-ahead if there is a gap in seq commands.