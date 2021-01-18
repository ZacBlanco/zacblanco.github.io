---
layout: page
title: Notes on "A Protocol for Packet Network Intercommunication"
keywords: CSE, 222A, UCSD, UC, San Diego, networks, packets, ip, reading, notes
mathjax: true
---


- Previous networks only considered machines within _one_ network
- seminal paper presents protocol design and philosophy for sharing across many interconnected networks.
- typical network composed of one or more hosts connected by one or more packet switches.

previously networks
- may have distinct ways of addressing receiver
- each network may accept data of different maximum sizes
- success or failure of a transmission and performance in each network is governed by varying delays.
- communication may be disrupted due to unrecoverable mutation of data.
- status, routing, and fault detection are different among networks

- much more convenient to have the same interface on all network devices
- should the interface account for differences between host or process?
- should play a small role, and there should only be one protocol that needs to be implemented on the system.
- interface between networks must play a central role as it is responsible for interconnection of multiple networks.

### The Gateway

- connect only two networks?
    - could be implemented as two halves to transmit packets between the networks it interfaces
- gateway needs to understand the source/destination hosts, information must be available in a standard format at the gateway.
- use the **Internetwork header**
- unless all packets are small enough, gateways the protocol must allow packet fragmenting in the case of a packet being too large.
    - if fragmenting is not allowed, technology could stagnate or features would be difficult to add because not all systems could be upgraded at once.
    - no global coordination required if supporting packet fragmenting
- argue gateway should not perform packet reassembly due to buffering issues


### Process-Level Communication

- suppose processes communicate with finite length messages that are served by a _Transmission Control Protocol_ (TCP)
- _TCP_ can break streams into segments and also accept sequence of segments to put together a message.
    - must consider out-of-order packets
    - another case, is that the packet should contain destination process information (a _process header_) that identifies the destination process.
- to decide if a packet is deliverable to the process, the TCP must determine if data is in the proper sequence - with sequencing and process information, the process identification is immediately available to the TCP for reconstruction.


### Address Formats, TCP Addressing, Port Addressing

- address selection is crucial, many network have substantially different address formats and sizes
    - similarly in process addressing
- split address into 8bit/16bit segments, with upper 8 identifying up to 256 networks, and the lower 16 as a TCP identifier. If the network destination isn't connected to the particular gateway, then route it to the next gateway
- use process "ports" to identify processes within a single machine. Destination host parses the port to determine process "mailbox"


### Segment and packet formats

- need _End Segment_ (ES) and _End Message_ (EM) flags on the packet for TCP.
- can re-interpret packets by merely scanning the header flags
- use a windowing method to determine retransmission vs new data
    - left window edge determines the point at which all bytes to the left of the window are received
- flow control

### TCP I/O

- new buffer allocated for each TCP arrival stream
- TCP could discard packets for which it doesn't have buffer space - eventually, they will be retransmitted.
- TCP needs its own header (TCP Control Block, TCB)