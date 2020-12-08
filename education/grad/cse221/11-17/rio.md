---
layout: page
title: CSE 221 - Operating Systems - Notes on "The Rio File Cache; Surviving OS Crashes"
permalink: /education/grad/cse221/11-17/rio/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, OS, fs, unix, rio, fs, filesystem, crash fault, tolerance
description: Notes for "The Rio File Cache; Surviving OS Crashes"
mathjax: true
---


> Q: Do you believe the underlying argument in Rio? Why or why not? Would you
> use a system that recovered data using Rio?

## Introduction

- published ASPLOS Oct. 1996
- prevent loss of memory on OS crash
- don't consider power loss due to UPS solution
- delayed writes - can work but risks losing data, and much is written to disk
  anyway, so not saving many disk IOs
- achieve perf by eliminating all reliability induced writes to disk, and
  achieve write-through reliability by protecting memory during a crash, then
  restoring


## Design and Implementation

- protection
    - make file cache a protected module in the kernel
        - need to make unlikely that non-file cache procedures corrupt mem.
    - method 1: ensure bit is set to make _all_ memory references go through the
      TLB
    - method 2: code patching, modify kernel to check before every load/store
      that address is not touching the file cache
        - 20-50% slower
    - program can still corrupt a file it has write access to.
- warm reboot
    - system needs to read file cache on a warm reboot, update system with the
    data.
        - need to know additional data system maintains - in order to perform
          restore.
        - when in reboot does system restore file cache?
    - maintain a registry of metadata containing information needed to find, identify and restore files in memory
        - 40 bytes per 8KiB page
    - dump all physical memory to swap partition first
        - user process analyzes memory dump and restores the UBC
- design effects
    - changes FS to write directly to cache and fsync return immediately.
    - writes to disk only when buffer cache overflows


## Reliability

- inject faults and test reboot process
    - flip random bits in memory
    - delete instructions, e.g. C-level programming errors
    - initialization fault; delete instructions responsible for initializing a pointer. place faults in malloc and bcopy
- detecting corruption
    - checksum file blocks with memory blocks
    - synthetic memtest workload to test direct/indirect corruption
- supposedly more fault tolerant than normal systems during crashes (no
  corrupted file data)


## Performance, etc

- basically as fast as RAM
- still vulnerable to hardware faults (e.g., memory board dies, bad die, etc)


## Lecture Notes

- goals?
    - performance of memory with reliability of disk
    - write-back performance with write-through reliability
- Why can Rio perform better?
    - eliminates reliability-induced writes to disk
- works because of a warm reboot; power not lost, just system reset
    - key requirement is that RAM is not reset.
- rio protects by virtual memory support
    - make file cache pages read only
    - need to ensure that all accesses to those pages use virtual memory protection
- comprehensive fault injection analysis
- paper ignored security -- not really adopted due to security issues