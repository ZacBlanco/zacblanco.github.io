---
layout: page
title: CSE 221 - Operating Systems - Notes on "The Distributed V Kernel and its Performance for Diskless Workstations"
permalink: /education/grad/cse221/10-27/distv/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, rust, memory, OS, distributed V, diskless, workstation
description: Notes for "The Distributed V Kernel and its Performance for Diskless Workstations"
mathjax: true
---


> Q: What is the argument for diskless workstations, and do you agree/disagree
> with the argument?

Answer:


## Introduction

- pub. 1983
- message oriented kernel
- provides network and IPC
- show diskless access with minimal performance penalty
- message passing facility is comparable to well-tuned and specialized remote
  file access protocols
- 3Mb ethernet
- argue that diskless stations
    - lower hw cost
    - simpler maintenance
    - little or no memory processing overhead
    - fewer problems with replication, consistency, distribution of files.

## Kernel IPC

- many small processes communicate via messages
-  use short ficed-length message that associate with a reply and data transfer
   option.
- message exchange of Send, Receive, Reply
    - any number of MoveTo or MoveFrom between message being received and reply
      being sent.
- All messages fixed 32 bytes.
    - send(message, pid)
    - pid = receive(message)
    - (pid, count) = ReceiveWithSegment(msg, segptr, segsize)
    - Reply(message, pid)
    - ReplyWithSegment(message, pid, destptr, segptr, segsize)
    - MoveFrom(srcpid, dest, src, count)
    - MoveTo(destpid, dest, src, count)
    - setPid(logicalId, pid, scope)
    - pid = getPid(logicalId, scope)

- synchronous req-response is cognitively easy to understand
- distinction between small msg and separate data transfer ties in well
with frequently observed usage
- synchronous communication and small fixed-size messages reduce mem. mgmt.
problems in the kernel
    - zero copy of data with data transfer messages

## Implementation Issues

- remote operations directly in the kernel
- inter-kernel packets use raw ethernet link level (IP has 20% increase)
- request-response pattern helps implement reliability with raw ethernet
- mapping from process id to location is aided by a host specification
- no per-packet acknowledgement for large transfers
- File page-level transfer require the minimum number of packets.

- process identifiers flat for entire network
    - unique within local network
- getPid uses network broadcast

### Remote Message impl

- If process is not local uses the _NonLocalSend_
    - remote machine receives a packet containing the request
    - remote kernel creates an _alien_ process descriptor to represent the
      remote sending process as a standard in-kernel descriptor
- original sender re-transmits packet after a timeout, T
- remote kernel can respond with packet, or a _reply-pending_ packet
  in the case no descriptors are available.

### Remote Data Transfer

- MoveTo/MoveFrom transmits the data in a sequence of maximally-sized packets to
the destination workstation.
    - single ack packet.
- MoveTo and MoveFrom, there is no queueing or buffering
    - basically one copy from memory to NIC, then NIC, to destination.
- full retransmission since last correctly received packet.
- page-level file access request involve intermediate amount of data.

### Remote Segment Access

- Previous protocol good for small amounts of data, or tens of packets
- Larger sized require a better protocol
- File access implemented with IO protocol originally developed for Verex kernel
    - For a two-packet (data sized) transfer, it requires 4 network messages.
        - Double of a specialized protocol
- Receive/ReplyWithSegment
    - To put request with data.

## Network Penalties

- cost of network is cost of local operation + cost of moving data across the
  network
- Generally, small
    - 8mhz cost (in ms): $$ P(n) = .0064n + .390$$
    - 10mhz cost (in ms): $$ P(n) = .0054n + .251$$
- Pair of machines on network can generate ~ 400kbit / sec
- Workstation limited to ~558 messages/sec due to processor speed.

## File access in the Dist. V Kernel

- read page with Send--Receive--ReplyWithSegment
- write page with Send--ReceiveWithSegment--Reply
- Page read/write remote is ~5.5-5.7ms
- Without segment message, cost increases to 8.1ms+
- for reasonable disk latencies, sequential file access is within 10-15%
- access latencies only really need to add network latency time.
- achieve closer latencies for program loading if you can cache programs in
  memory

## File Server Issues

- estimate approx. 28 file requests per second. -> approx 10 workstations.
- improve system by adding another file server
    - network not the bottleneck for < 100 systems.
- File server should have general program execution capabilities.
- did not address any security concerns.


# Lecture Notes

- SUN - Stanford University Network
- All IPC in the V kernel is based on generic message-based interface
    - based on Toth OS from Cheriton
- paper addresses concern that kernel IPC might not be suited for distributed
  OS.
- why appear inefficient?
    - short message make inefficient use of large packet sizes
    - synchronous communication prevent overlap of net I/O and computation
    - separating data transfer from control messages increases number of network
      operations.
        - Addressed by implementing two new message primitives
          (Send/ReceiveWithSegment)
- Use raw ethernet frame directly in kernel for lowest overhead
    - sync req/resp used to build reliable datagrams on unreliable frames
        - reduce buffering
    - no per-packet acknowledgements for file page-level transfers
- Network penalty
    - minimum time to transfer datagram from one workstation to another
    - includes processor time to transfer the datagram, time to transmit
    - measures overhead of interposing network into a local communication.
- argue random remote file access adds only minimum penalty
- streaming
    - two level streams: one in network layer, another is disk I/O
    - argue neither is necessary