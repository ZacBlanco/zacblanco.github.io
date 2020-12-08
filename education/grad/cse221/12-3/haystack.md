---
layout: page
title: CSE 221 - Operating Systems - Notes on "Finding a needle in Haystack; Facebook's Photo Storage"
permalink: /education/grad/cse221/12-3/haystack/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, OS, facebook, photo, storage, haystack
description: Notes for "Finding a needle in Haystack; Facebook's Photo Storage"
mathjax: true
---

## Introduction

- FB has lots of photos --> 60+PiB
- 1M images/sec served at peak
- object storage system
- usage semantics: write once, read a lot, never delete
- key issue is the need to read disk to lookup file metadata
    - too much latency in NAS systems
- system must be high throughput/low latency, fault tolerant, cost
  effective, and simple.
- CDNs alone are not effective enough
    - best at serving hottest photos, but not for older content.
    - long tail latencies
- Original design CDN was cache for a large set of commercial NAS appliances
- thousands of files per directory (lots of disk ops for single image)
- no existing systems are suitable for the type of photo workload FB has
- Design after haystack still uses CDN
    - most popular images get cached
    - haystack reduces long tail latencies.
- haystack designed to reduce disk operations

## Haystack Architecture Overview

- 3 components
    - haystack store
    - directory
    - cache
- "store" is persistent storage
- store capacity divided into physical volumes.
- Physical volumes grouped on machines called logical volumes
- photo written to a logical volume written to all physical volumes
- directory maintains logical to physical mapping
    - additionally, application metadata such as where photos reside.
- Cache functions as an internal CDN.
    - insulation in case of CDN failures
- directory does not communicate with cache or store.
- location is URL encoded
    - `htto://<CDN>/<cache>/<machine id>/<logical volume, photo>`
- The haystack directory
    - 4 functions
        - mapping of logical to physical volume
        - directory load balances writes across logical volumes and
          reads across physical volumes
        - determines whether a photo request should be handled by CDN or Cache
        - identifies logical volumes that are read-only due to
            operational reasons or because they are at max capacity.
            - volumes marked read-only at machine granularity.
    - implemented with PHP with memcached in front.
- haystack cache
    - receives HTTP requests for photos from CDNs and also directly from
      browsers
    - implemented as a DHT.
    - Photo ID is the key.
    - read through cache to store if not available
    - caches only if the request comes directly from a user, and the
      photo is fetched from a write-enabled store machine.
    - Cache shelters write-enable store machines from reads
        - photos are most heavily accessed soon after they are uploaded
        - FS generally perform better when doing reads OR writes, but not both.
- haystack store
    - very basic interface
    - each store machine manages multiple physical volumes. Each volume
      holds millions of photos
    - physical volume basically stored as one huge file on disk.
    - quick access with id of file and file offset of photo.
    - keep open FD for each physical volume managed, and a mmap of photo
      ID to fs metadata.
    - maintain in-memory data structure which maps pairs of (key alternative key) to a corresponding set of needle flags, size in bytes, and volume offset.
    - after a crash, a store machine can reconstruct this mapping from the volume file before processing requests.
    - directory assigns cookies to request to prevent id guessing
        - verify ID before return
    - writes
        - web server provides logical volume id, key, alt key, cookies and data to the store.
        - machine synchronously appends needle images to physical volume files and updates in-memory mappings as needed.
    - deletes
        - set delete flag in metadata and in volume
        - error if retrieve needle with delete set
    - index file
        - index file used when rebooting
        - prevent long startup - otherwise need to read TBs of data
        - index files update asynchronously.
            - represent stale checkpoints.
            - writes/deletes always change to volume synchronously but
              update index asynchronously.
        - needles without index records are orphans
            - on startup, add records for orphans
            - orphans identified because volume is built like a stack.
- use the XFS filesystem to prevent additional disk operations (store block info in memory)
- optimizations
    - compaction
        - periodically scan to remove deleted photos and shrink volumes
    - save memory
        - xfs inode is 536 bytes, haystack uses only 40 bytes of
          metadata per photo (even with 4x copies for different sizes)
    - batch upload
        - improve perf by writing lots of data at once -- allow multiple
          uploads as albums


## Lecture Notes

- Stores 260B images, PBs of data
- 1B images each week
    - 60TB/week.
- in 2019, 300M photos/day (Source Gizmodo)
- two types of workloads
    - profile pictures (heavy access, small size)
    - photo albums - intermittent access, high at beginning, decreasing over time
- each image in own file --> huge amount of metadata per image
- amount of metadata far exceeds NFS caching tier abilities
- high reliance on CDN --> expensive
- haystack advantages
    - 10TB/node --> 10GB metadata
        - easily cacheable
    - simplified metadata
        - much less metadata than FS inode
- Haystack cache - DHT
    - photo ID to find cached data
    - processes CDN+browser requests
    - must meet two conditions to add to cache (see rules above)
- consider - if different use case, how to design? (e.g. snapchat)
- consider other abstractions (Album?)
    - important/better if photos from same album are placed sequentially or close together
- privacy concerns
    - photos sufficiently protected?
    - security within FB? how enforced?
    - does photo deletion respect user privacy concerns?