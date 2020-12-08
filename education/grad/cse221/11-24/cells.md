---
layout: page
title: CSE 221 - Operating Systems - Notes on "Cells; A Virtual Mobile Smartphone Architecture"
permalink: /education/grad/cse221/11-24/cells/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, OS, cells, smartphone, architecture
description: Notes for "Cells; A Virtual Mobile Smartphone Architecture"
mathjax: true
---


## Introduction

- ease of downloading new software poses a risk of leaking information
  to third parties
- ARM is mainly on phone - not virtualizable
- no direct leverage of devices from within VMs
- use a "virtual phone" (VP) concept to restrict applications
    - union FS to share code.
    - user-level device namespace for multiplexing hardware


## Usage Model

- foreground VP always rendering, background VP not rendering, but still
  capable of processing.
- VP configured with different access rights to particular devices.
    - allow none, shared, or exclusive access
- use of some VM technology to prevent privilege escalation attacks.


## System Architecture

- virtualized resource identifiers to prevent leakage of information to
  another VP
    - use linux namespaces
- three methods of implementing namespace functionality
    1. create a device driver wrapper using a new device driver for a
      particular virtual device.
        - wrapper multiplexes communicates on behalf of application to
          real driver
    2. modify the device subsystem to be aware of namespaces
        - without namespacing, device input sent to all namespaces
          listening. Need to implement multiplexing.
    3. modify the device driver to be aware of device namespaces
        - Binder IPC mechanism should not be shared by any other VP
        - modified to only list processes within namespace

- User-Level Device Virtualization
    - cells boot into minimal root namespace
    - custom process for switching VPs
- Scalability and Security
    - 3 scalability techniques
        - same base file system is shared and RO
        - when a new VP is initiated, use kernel samepage merging 9KSM)
          to reduce memory usage.
        - utilize android low memory killer to increase VPs
    - 4 techniques to isolate VPs
        - user credentials are virtualized through UID namespaces
        - kernel device NS isolate device access
        - mount namespaces provide a unique and separate FS view to each VP
        - CellD removes capability to create device nodes inside a VP,
          preventing a process from gaining direct access to linux
          devices
- Graphics
    - relies on linux framebuffer to provide abstraction of physical display
        - screen memory mapped and written directly by processes and hardware.
    - background VPs simply write to a virtual FB, even though it is not displayed
    - foreground VP gets exclusive access to the GPU FB
    - device driver is basically a hw multiplexer, but only changes when
      FG VP changes.
    - 4 steps to changing FG renderer VP
        - memory remap, screen memory deep copy, hardware sync, gpu coordination
- power management
    - background VP should not be able to put device in low power mode
    - background VP should not prevent the foreground vp from putting the device into a lower power mode.
    - make `gbearlysuspend` call namespace aware to ensure device isn't put into
      low power mode.
    - wake locks
        - Is kernel ref counter with _active/inactive_ states.
        - locked = active
        - unlocked = inactive
        - semantics kind of like RW lock: many locks make it active, single
          unlock though makes it inactive.
        - need to make sure unlocks from non active VP aren't propagated to the
          actual system. Must keep track of namespace.
- telephony
    - virtualize radio stack.
    - VP library communicates to the root namespace, which communicats to the
      driver, and finally the baseband telephony device.
- networking
    - kernel and user-level virtualization for necessary isolation
    - use network namespaces to virtualize addresses
    - virtual ethernet pairing to send requests+NAT
    - configuration different for wpa_supplicant in all VPs
        - switch config when fg-bg VP switches

## Lecture Notes

- main point: support multiple virtual phones on same physical hardware
- not a true virtual machine
    - virtualize as the OS interface
- foreground VP displays VPs
    - remap OS resource IDs to virtual ones
- framebuffer multiplexer most difficult
    - foreground VP goes to real FB
    - background VP frame buffer goes to RAM