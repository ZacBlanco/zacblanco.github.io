---
layout: page
title: CSE 221 - Operating Systems - Notes on "The Sprite Network OS"
permalink: /education/grad/cse221/10-27/sprite/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, rust, memory, OS, distributed, sprite, network
description: Notes for "The Sprite Network OS"
mathjax: true
---


## Introduction


- Authored 1987
- Sprite is an OS to design high performance multiprocessor workstation
  with hardware support for Lisp applications
- three general trends by authors
    - networks
    - large memories
    - multiprocessors
- computing on LANs are common
- workstations tend to suffer from poor performance and difficulties of
  sharing and admin
- current memories of 4-32, expect 100-500 later on
- imminent arrival of multiprocessor workstations
- goal is to provide a simple and efficient mechanism to capitalize on
  the trends
- kernel calls similar to BSD, but include three new facilities for sharing
    - transparent network fs
    - a simple mechanism for sharing writable memory for processes on a
      single workstation
    - mechanism to migrate processes between workstations to take advantage of
    idle machines
- caching done in memory and locally (e.g. store remote file in local cache)


## Application Interface

- ultimate goal of network transparency
- access any device regardless of location
- Proc_fork shares data segments
    - all or nothing sharing
- expect multiprocess application to use hardware mutex (e.g.
  test-and-set) directly on shared memory
- Proc_migrate to move a process (or group of processes) to another machine
    - done via shell commands
- remote shell commands (e.g. ssh) not really implemented yet.
- process migration is key to utilizing unused network resources
    - key difference is the processes are _already running_

## Kernel Structure

- key features are a multi-threaded synchronization structure, and an
  RPC facility
- several processes may be in the kernel
- implement RPC as stubs and transport
    - stub transforms args into request msg
    - transport puts it on the network.
- wished there was an automated stub generator
- assume a secure network
    - no encryption
- minimum RTT 2.8ms

## Prefix Tables

- need to manage file namespace to simplify system admin
- manage data in a way to provide high performance
- don't compromise simplicity of sharing files
- imagine fs as a single tree, but there can be subtrees in different domains
    - domain represents another server
- manage the namespace as a set of prefix tables, and each entry in the
  table corresponds to a domain
- To locate a file
    - Match the name against all entries of the prefix table, choose the entry
      with the longest prefix
- open directory and store a token and server address with the process to make lookups on relative entries quicker
- there is a "root" server for the fs
- prefix approach bypasses looking up on root server, so it reduces the maximal
load
- update tables with IP broadcast entries
- prefix table entries are like hints - can be updated and adapted on server
  crashes or errors.

## Managing File Data, Client and Server Caches

- every server has a large cache of recently accessed file blocks
- caches organized on block rather than whole file to store in main memory.
- 4k blocks.
- Reads always check the local cache first, then try to read the block
    - new blocks added to cache via LRU.
- writes are handled via _delayed writes_
    - write block to cache first, then return to the application
    - block not written to disk or server until ejected from cache or 30 seconds
      elapsed.
- Sprite guarantees full consistency with version numbers
    - when opening a file, server returns current version for the file
- Keep track of last writer to identify location of an updated file
- concurrent writes disable caching
- very slow file access when caching is disabled

## Virtual Memory

- mostly traditional virtual memory system
- backing storage by using a portion of disk to hold pages wapped out of
  physical memory - traditional UNIX
  - difference w/ sprite is the use of general files as backing store
  (instead of a whole device)
  - implications for remote paging device
- sticky segments
    - pages for programs remain in memory until replaced using the normal page
    replacement algorithm
- double caching
    - virtual memory bypasses local file cache to read and write backing files
    - servers only cache backing files for _other_ clients, not for their
    own data.
- file cache grows and shrinks in response to change of virtual memory demand
- page replacement occurs in oldest of file cache _or_ virt. mem

## Process Migration

- two differences to previous approaches
    - mechanism in which vm of a process is transferred between machines
    - the way in which migration is transparent to a migrated process
- simple approach
    - 1 freeze process
    - 2 transfer state to the new machine (registers, mem, file access, etc)
    - 3 unfreeze
- vm is dominant migration cost
- backing files simplify transfer the vm image to another machine, old machine
  just pages out the process dirty pages
- requires less total data, and old machine does not need to service page faults
- processes get a home node
    - results of some kernel calls dependent on home kernel are routed to home
      node
- transfer state of the home node over to prevent needing to send data
    - otherwise, RPC


## Conclusions

- 100k LoC
- Mostly C, few hundred in assembly


# Lecture Notes

- Seminal dist. FS
- Sprite is a network OS with main goal of location transparency

- 3 tech trends
    - networks:
        - collection of distributed workstations
        - hide distribution
        - single image system
    - larger memories
        - large caches
    - multiprocessors
        - OS can be multiprocessor aware
        - previously, kernels had very coarse-grain locking
            - linux didn't have good multiprocessor support until 2004
- application interface
    - single uniform namespace
    - shared memory for processes
    - process migration

- Two interesting aspects
     - support for multiprocessor
     - support for RPC's
        - RPCs are internal to the kernel, not exposed to application
- Prefix tables
    - map filesystem prefixes to domains
    - Compared to NFS, completely dynamic w/ broadcasts
        - NFS very static
    - broadcast if receive an error or don't have entry for a domain
- Caching uses delayed writes to improve performance and exploit memories
    - dist v. didn't want to do caching because of consistency problem
- Consistency with versioning of files
- synchronization with fs locks
- virtual memory swaps with files as the backing store
    - files give:
        - uniform naming
        - facilitates process migration
        - aggregate backing store for multiple clients onto one machine
        - don't need to commit separate disk space to backing store
        - can cache client backing store pages in server cache
    - also designed to avoid double caching as much as possible
    - Can reclaim dynamically which pages/files are evicted from the cache
- summary
    - biggest contributions are
        - file caching protocol
        - dynamic balance between VM and file buffer cache
        - arguments about large client and server caches
    - long history of thin clients
        - TTY, IBM terminal, X term, V Kernel, Sprite/Sun, Oracle ThinClient
    - Why are thin clients not used much today?

