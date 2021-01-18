---
layout: page
title: CSE 222A - Networks
permalink: /education/grad/cse222a/
keywords: CSE, 221, UCSD, UC, San Diego, networking, tcp, udp, ip, software, operating, system, linux, C, rust
description: Notes for CSE 222A (Network Systems) at UC San Diego
mathjax: true
---


## Lecture 1 - Course Intro

- Networks have stood the test of time -- why do more?
    - requirements changing!
        - billions of IoT devices
        - new usage (5G/mobile)
        - applications (device control, VR/AR)
        - state-level threats
        - policies anonymity/accountability


## Lecture 2 - End-to-End Arguments

- Where to implement particular functionality?
    - reliability
        - must be in the application
    - security
        - should the application also do this entirely?
        - in network-provided encryption where OS handles encryption, anyone in between app and OS --> can see unencrypted data
    - routing/addressing
        - not absolutely necessary for app?
        - application doesn't really care how it gets to the destination
    - movement of bits
        - 3 pieces of equipment: medium, transmitter, receiver

- applications have tons of "moving" parts:
    - disks, OS, filesystem, ram, links, power, cpu
    - should we make them all super reliable....or super unreliable?
        - only implement reliable system underneath if you can increase
          performance

## Lecture 3 - Internet Routing

- Interconnecting generic networks
    - main challenge is heterogeneity of link layers
        - addressing
        - bandwidth
        - latency
        - frame size
        - loss rates
        - service guarantees
- routers forward packets from source to destination
    - may cross many separate networks along the way
- all packets use the common _internet protocol_
    - any underlying data link protocol
    - any higher layer transport protocol
    - IP is the "thin waist"

- router: store and forwards
    - connected to multiple networks
    - supports multiple data-link layers and makes decisions
    - must be explicitly addressed by incoming frames
    - parses IP header to determine next-hop


- brief history of routing
    - arpanet with dynamic distance vectors
    - new networks on the ARPAnet (civilian vs military)
        - NSFNet, etc
        - nodes grew exponentially
        - networks with different rules
    - new requirements
        - need to scale to millions of routers
        - varying route metrics
        - must express business policies
- shortest path is _not_ always the best path
    - must account for "link cost"

- to respect routing rules - need routing through outside entities
  separate from within particular domains
  - routing within a domain uses traditional protocols (RIP, OSPF)
  - routing between domains uses exterior gateway protocols (EGPs)
    - only have reachability information, decide routing based on local
      policy

- Autonomous Systems
    - internet divided into separate ASes
    - distinct regions of administrative control
    - routers/links managed by a particular institution
    - service provider, university, company, etc
    - disjoint hierarchy of ASes
        - tier 1: national backbone providers
        - tier 2: medium sized regional provider w/ metro-area affinity
        - tier 3: small network w/ single company, university, etc
- inter-domain routing
    - border routers summarize and advertise routes to neighbors
    - border routers apply policies
    - internal routers can use a notion of **default routes**

- path-vector routing
    - give advertise known paths to neighbor ASes
        - prevents loops
    - problems with BGP (path vector implementation)
        - instability (route flapping when a link goes down)
            - long AS-path decision criteria causes DV-like behavior
            - not guaranteed to converge
        - scalability still difficult
            - > 500k network prefixes in default-free table
            - tension: want to merge traffic to specific networked/multi-homed networks
        - performance
            - not always optimal and not balanced across paths

## Lecture 4

BGP Routing
    1. get routes from neighbors
    2. select the routes to use
        (route selection not defined in BGP)
    3. decide routes to share with neighbors

- Three attributes of routing graphs
    - convergence
        - all routers have same information about routes that exist
        - agree on set of routes
    - stability
        - no AS changes its route based on what it knows now
    - safety
        - no matter what happens, we will eventually converge

## Lecture 5

- problems with internet
    - abstractions between theory and reality
    - human error (and animal threats!)
    - policy issues
- for routing paper...why 37 sites?
    - symmetric routes
    - try to get a representative coverage sample across the entire internet
        - only touched 8% ASes...not very many, but "a lot" for any study at the time

- routing loops in the wild
    - occurs across national borders!
- fluttering - 50/50 across links?
    - no rules?
    - also, e2e argument -- splitting routing for performance benefit

- dominant routes
    - prevalent vs persistent paths
        - prevalent paths: most take one path
        - persistent paths: amount of time spent between switching paths
    - found some were very stable
        - others, switched quite often
        - others neither prevalent or persistent