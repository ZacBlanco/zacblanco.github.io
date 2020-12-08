---
layout: page
title: CSE 221 - Operating Systems - Notes on "TaintDroid; An Information-Flow Tracking System for Realtime Privacy Monitoring on Smartphones"
permalink: /education/grad/cse221/11-24/taintdroid/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, OS, cells, smartphone, architecture, taintdroid, privacy
description: Notes for "TaintDroid; An Information-Flow Tracking System for Realtime Privacy Monitoring on Smartphones"
mathjax: true
---


## Introduction

- realtime tracking and tagging of private information within a phone to
  log use of data
  - needs to be low performance impact
- 2/3 of apps send data suspiciously
- challenges
    - phones are resource constrained
    - 3rd party apps are entrusted with several types of privacy sensitive information
    - context-based privacy sensitive information is dynamic and can be
      difficult to identify
    - applications can share information.
- instrument the VM interpreter to provide variable-level tracking
  within untrusted application code.
- maintains taint markings for data and not code
- message-level tracking between applications
- method-level tracking for system-provided libraries
- assumes firmware code is trusted
- can't be used on libraries which require their own .so file
  distributed with the application.

- Android:
    - applications compiled to Dalvik VM bytecode
        - register-based machine language
    - access to native libraries
    - Binder IPC to communicate between processes

- TaintDroid
    1. information tainted with context
    2. taint interfaces invokes a native method
    3. VM propagates the taint tags from vm interpreter
    4. modified binder IPC library tracks tainted data tags
    5. parcel with taint is passed through kernel and received by application
    6. modified binder receives parcel message with taint tags, binder library unpacks this
    7. Dalvik VM gets taint tags from message and assigns to process.
- challenges
    - where to store tain tags?
    - how to propagate?
    - native code taint propagation
    - IPC taint propagation
    - secondary storage taint propagation
- tag storage
    - instead of byte-level semantics, use variable-level semantics
    - 32-bit vector with each variable to encode the taint tag.
    - allocate taint storage by doubling the size of the stack
    - bad for arrays, because requires taint tag on all arrays. Hard to
      avoid otherwise false positives are likely
- interpreted code taint propagation
    - large list of operations for which each a taint propagation needs
      to be specified depending on the arguments and any particular
      taints that they contain.
- native code taint propagation
    - not currently tracked
    - best can do is external variables are assigned taint tags according to
      data flow rules, and return value assigned by a taint tag according to
      data flow rules
- IPC Taint propagation
    - message-level taint tracking on application IPC
- Secondary Storage Taint Propagation
    - Only one taint tag per data file. Some tags may be lost when writing, but
      at least one will remain.

- Privacy Hook Placement
    - Place tag generates at the edge of devices which generate private
      information
    - types
        - low-bandwidth sensors, e.g. location and accelerometer
        - high bandwidth sensors: micriphone, camera
        - information DBs: SMS, address books
        - device identifiers: SIM ID, IMEI, etc
        - network taint sink: add log when taint reaches the device edge via
          network wifi or cellular signal
- Study
    - select 30 popular applications to test tainting
    - used only wifi to transmit packets
    - noted whether apps asked for user consent to transmit sensitive info
    - flagged 15 applications sent info to ad servers, 7 sent device IDs, and 2
      sent phone information such as location and phone number.
    - CPU and memory footprint within some small margin. 10-20% in most cases.
      Memory footprint was smaller than CPU footprint.
- Other
    - no taint tracking on direct buffer objects
    - significant false positives when tracked information contains
      configuration identifiers. e.g. the IMSI numeric string.

## Lecture Notes

- Main point - show feasibility of taint tracking in Android
- How does it work?
    - identify sensitive data
    - taints tracks and data flow with
        - variables, messages, methods, and files
    - monitors behavior of running apps in real-time
    - identify misuse of private data
    - generally - not very practical
- two ways to propagate information
    - explicitly => direct transfer from one object to another
    - implicit => indirect transfer due to control flow
- use highly optimized Dalvik VM
    - lots of mem sharing and COW to prevent too much memory usage
- perf impact
    - 14% perf overhead
    - 27% lower IPC (more memory tagging ops)
    - 3.5% more memory