---
layout: page
title: CSE 221 - Operating Systems - Notes on "Implementing Global Memory Management in a Workstation Cluster"
permalink: /education/grad/cse221/10-29/gmm/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, OS, distributed, memory, management, workstation, cluster
description: Notes for "Implementing Global Memory Management in a Workstation Cluster"
mathjax: true
---

> Q: How does the implementation of the GMS page replacement algorithm
> approximate the ideal algorithm?

Answer:

## Introduction

- Idea that machines should share memory together
    - currently each machine exposes service, but leaves resources available
- nodes can leave/join at any time
- no globally managed data lost if a node crashes


## Algorithm

- Node P on page fault performs the following global replacement algorithm
    - **Case 1**: faulted page is in global memory of node Q
        - swap desired page in Q's memory with any page in P's memory
        - P's local memory increases by 1, global decreases by 1
        - Q's balance is unchanged
    - **Case 2**: The faulted page is in the global memory of Q but P's
      memory only has local pages
        - exchange an LRU local page on P with faulted page on Q.
            - Size of global memory on P and Q are unchanged
    - **Case 3**: Page is on disk
        - read faulted page in P's memory to become a local page
        - choose oldest page in the whole cluster for replacement
            - write to disk if necessary
        - send global page from P to node Q where it continues as a global page.
        - if P has no global pages, choose P's LRU local page instead
    - **Case 4**: Faulted page is a shared page in the local memory of another
      node Q
        - Copy shared page into a frame on node P, leaving original
          memory in tact
        - Choose oldest page in the cluster (maybe on node R?) for replacement
            - write to disk if necessary
        - send global page on P to R where it becomes a global page.
            - use P instead if P has no global pages.
- With algorithm, local:global ratio changes dynamically
- ultimately, minimize cost of memory references
- time divided into epochs
    - new epochs begin after a time T, or a number of page replacements, M
- tradeoff between keeping information up to date and manageability of global
  info
- Change the values of M and T based on velocity
    - lots of old pages, T can be larger
- node failures do not cause data loss in global memory b/c global memory pages
  are clean

## Implementation

- inserted calls to GMS module in kernel when pages are added+removed
- inserted calls to VM swapping to track additions and deletions
- Data structures
    - PFD (page-frame directory)
        = directory of pages on node
    - GCD (global-cache directory)
        - locate ip address of node with a particular page
    - POD (page-ownership directory)
        - UID of shared pages to node storing the GCD section with the page
- new node added by checking in to designated master node
- two step lookup
    - use UID of page to hash into pod, produce IP of node
    - then use UID on GMS on node to request the page
- collecting local age information
    - modified TLB handler - once a minute, flush TLB of all entries
        - when TLB performs a translation on a miss, set a bit on that frame
        - kernel thread samples per-frame bit on every period to maintain LRU
          statistics
- assume reliable network
    - no dropped packets ever experienced?? :o
- access times of ~1.45ms/1.55ms on miss/hit respectively
- workloads speedup of up to 4x using idle memory


## Lecture Notes

- reason for covering:
    -  uses a probability-based algorithm, a potentially powerful tool for
       distributed systems
    - probability based algorithms can enable multiple independent entities to
      make local independent decisions when combined, achieve a goal
        - another system to cover soon uses a randomized algorithm (lottery
          scheduling)
- cluster of machines connected on LAN
- goal to use underutilized resources
- page over network instead of disk
- interesting part is global page replacement algorithm
    - network faster than disk for paging
- model
    - two pages: local+remote
    - global pages stored on behalf of other nodes
    - global memory is another level of memory hierarchy
    - 4 cases (see notes section)
- 3 main aspects of implementation
    - maintaining page ages
    - finding pages in cluster
    - making replacement and eviction decisions
- how to determine global page ages?
    - use epochs
- how to determine local page age?
     - file pages are simply; need to track accesses to mapped pages
     - use similar trick as in Ozalp paper
        - invalidate TLB entries
        - TLB misses determine when pages are accessed
        - doesn't work in x86 because in x86 TLB miss handled by hardware - in this paper, OS handles the miss
- making replacement decisions
    - divide time into epochs
    - in an epoch, replace M oldest
        - in beginning of epoch, decide how oldest M pages are distributed
    - send all info to a central node
    - each node was W_i fraction of oldest pages
    - when a node needs to decide where to evict
        - choose node randomly weighted by W_i
    - decision to evict is made locally
    - after M evictions, on average, will evict M oldest pages in cluster