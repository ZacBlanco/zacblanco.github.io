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

## Lecture 6

- simulation vs prototyping
    - sally floyd-- likes simulation, vern likes internet measurement
    - internet is meshing of physical engineering and software engineering
- simulation gives "an intuition"
    - deployment on internet can't really be updated once deployed
    - easy to test parameters in simulation
    - deploy vs simulate?
- torrenting used to be big traffic hog (now it's netflix)

## Lecture 7

- internet architecture generally secret (company proprietary, national
  security)
- measuring the _entire_ internet with all the probes takes way too long
- CPUs of routers are vulnerable to traceroutes
- many different network architectures depending on the provider
- lots of redundant links
- providers generally have the same set of backbone nodes
    - e.g. ATT hub and spoke might be more prone to failure


## Lecture 8

- 1970s...
    - Network Security - review of network security
    - original TCP/IP design with trusted networks and trusted hosts
    - E2E principle -- intelligence at the edges
- 1980s
    - wait...we can't trust everyone
    - can't trust hosts; compromised, untrusted insider, anyone can connect to
      the public internet
    - network itself still trusted
- ..today
    - can't trust the network either
    - network equipment may be compromised
    - untrusted network operators
    - anyone can access physical channel of wireless networks
- TCP/IP
    - built in trust assumptions
        - net protocols only used as intended (assume correct packet headers and/or consider of other resources)
    - hosts are controlled by trusted administrators
        - random people can't get onto the network
        - correct information reported by hosts
        - protocols implemented correctly.
- Attacker models
    - MITM
    - Passive: eavesdrop on traffic
    - off-path: attacker injects traffic into network
- no confidentiality
    - who can see packets you send?
        - networks (router/switch, AP, ...)
        - unprotected wireless network
        - WPA2 PSK: everyone on same network
        - non-switched ethernet: everyone on network
        - switched ethernet: maybe everyone on same network
- no authentication
    - TCP/IP does not offer packet authentication
    - attacker with direct access to network can spoof source addresses
    - connectionless protocols are especially vulnerable
- link layer (MAC)
    - physical channels often shared by multiple hosts on local network
    - link layer controls access to the physical medium
    - how to make sure each host only gets frames addressed to it
    - each host is responsible for picking up frames addressed to it, ignoring
      others
    - filtering typically happens on the network card
    - many support "promiscuous" mode - pick up all frames
    - ARP spoofing
        - attacker on network can impersonate any other hosts
        - mitigation: fixed ARP tables, port binding on switch, higher level
          auth.
- problems with addressing
    - how to know what the "real" address is?
    - domain name to IP
- BGP
    - each BGP node needs to maintain connections to a set of trusted neighbors
    - neighbors share routing information
    - no authorization
        - malicious nodes can provide incorrect routing that redirects IP
          traffic
    - BGP hijacking
        - pakistan tried to block youtube within the country
            - pakistan claimed ownership of youtube's IP block via BGP
            - BGP nodes forwarded the routing information
            - youtube sinkholed globally
        - myetherwallet compromised, $100K+ stolen
            - attacker used BGP hijack to claim ownership of AWS route 53 DNS
              addresses
- IP spoofing attacks
    - no auth on link layer, TCP more complicated
    - connectionless protocols is simple
- DDoS attack w/ spoofing
    - millions of packets at some victim with forged source address
    - hard to block+identify actual source
    - reflector attacks
        - identify online services that generate large response packets in
          response to small requests (e.g., DNS, memcached, NTP)
        - send requests to such services with source address of victim
            - huge bandwidth amplifier
    - partial fix:
        - source address validation
        - edge routers filter outbound packets with source address that could
          not be generated within a network
- Denial-of-Service
    - attack against availability, rather than confidentiality, integrity, etc
    - two kinds:
        - logical vulnerabilities
            - ping of death, Land -- fix w/ filter+patching
        - resource consumption
            - overwhelm with spurious requests (SYN flood, smurf, bandwidth,
              overflow)
- resource consumption of services
    - Server CPU/memory resources
        - consumes connection state, time to evaluate message, cause new
          connections to be dropped, DB to query a lot
    - network resources
        - many routers packet-per-second processing limit, FIFO queueing
        - if attack is greater than forwarding capacity, good data will be
          dropped
- Address spoofing 2
    - puzzles: don't commit state until client has done a bunch of work for you
      (solve a computationally tough problem)
        - server provides puzzle to client
        - client must solve puzzle
        - tricky if validation isn't super cheap
    - traceback
        - router support for tracing packets back to their source
        - probabilistic packet marking
        - packet logging

## Lecture 9

- attacks like SolarWinds and WannaCry still persist
- networked computers have inherent trust
- can't really prevent an attack very well once started - especially before
  people notice
- bug with code red propagation - used a fixed random seed for ip propagation
- admins and orgs should diversify


## Lecture 10

- zmap challenges view of scanning internet as bad
- opened lots of new opportunities
- SYN-cookies to put state in the sent-packet
    - packets coming back have same information as sent packet

## Lecture 11

- methods to share physical media: multiple access
    - fixed partitioning
    - random
- channelizing mechanisms
- contention-based mechanisms (Aloha)
- wireless channel problems

- fixed partitioning
    - need to share media with multiple nodes (n)
        - multiple simultaneous conversations
    - a simple solution: divide channel into multiple separate _channels_
    - channels are physically separate
        - bitrate of link is split across channels
- Frequency Division Multiple Access (FDMA)
    - divide bandwidth of _f_ Hz into N channels each with bandwidth of $$f/n$$
    - easy to implemented, but unused subchannels are idle
    - used by traditional analog cell phone service, radio, TV
- TDMA (Time Division Multiple Access)
    - divide channel into rounds of _n_ time slots
    - assign hosts to particular time slots within round
    - unused slots idle
    - used in GSM and digital cordless phones
- Code Division (CDMA)
    - do nothing ot physically separate channels
    - all stations transmit at the same time in same frequency bands
    - "spread spectrum techniques"
    - "split up energy"
- problem with channel partitioning
    - not well suited from random access usage
    - instead, design schemes for more common situations
        - not all nodes want to send all the time
        - don't have a fixed number of nodes
    - potentially higher throughput for transmissions
        - active nodes get full channel bandwidth
- Aloha:
    - designed in 1970 to support wireless data connectivity (hawaii)
    - goal: distributed access control
    - aloha in a nutshell
        - send when data is ready
        - if transmission not successful, retransmit after fixed delay
- elements of a wireless network
    - infrastructure mode:
        - base station connects mobile devices into a wires network
        - handoff: mobile changes changes base station when signal strength
          changes
    - ad-hoc mode:
        - no base stations
        - nodes can only transmit to other nodes in link coverage
        - nodes organize themselves into a network
- wireless link characteristics
    - difference from wired links
    - decreased signal strength over distance
    - interference from other sources
    - multipath propagation
        - radio signal reflects of objects
- wireless link characteristics
    - achieve particular bit error rate based on SNR and transmission rate

## Lecture 12

- not many lecture notes...see paper summary notes
- exponential backoff when there is interference.

## Lecture 13

- mesh networks - hard to guarantee connectivity
- network worked pretty well
- why was their result surprising?
    - packet loss 20% median loss rate
    - interference also plays a role
    - more dense deployments, creates higher packet loss rates
    - data rates with higher loss is better than lower data rate with less loss
    - more losses doesn't mean bitrate is wrong..just that your network is dense


## Lecture 14

- Peer to peer networks
    - generally, each node is equal and join together to provide a service
- p2p adoption:
    - client-client file sharing
    - digital currency
    - voice/video telephony
- classic p2p: bittorrent
    - user clicks download link: get torrent file with content hash, IP of tracker
    - user's BT client talks to tracker, tracker tells peers who have file
    - user BT client downloads file from one or more peers
    - B client tells tracker it too, has a copy
    - users BT client serves files to others
- flat-name lookup problem - the true problem behind P2P
    - one solution: semi-p2p where content is distributed, but index is in a DB
        - e.g. Napster. Single DB for index, clients store data
        - O(N) state stored in single server
        - SPOF
    - query flooding: gnutella:
        - ask neighbors for content, they ask neighbors for content, etc...
        - doesn't scale for large networks..
    - routed DHT queries (Chord)
        - can we make the p2p system robust, maintain reasonable state per node
          and latency?
        - local hash table: hash, put, get
        - DHT: hash(data), lookup(key) --> IP Address, sendRPC(IP, put, get, key, data), sendRPC(IP, get, key) -> data
        - partitioning of data in a truly large-scale system
        - tuples in a global database engine
        - data blocks in a global FS
        - Files in a P2P file sharing system
- Why are DHTs hard?
    - decentralized
    - scalable
    - efficient
    - dynamic
- consistent hashing


## Lecture 15

- Pastry - DHT (key-location network)
    - joining -- need to "know someone"
    - security issues: must trust first node in the network
- failures: designed to survive L/2 failures (leaf failures)
    - tolerate failures by replicating values to nearby leaf nodes
    - byzantine nodes would need to take over multiple leaf nodes that have
      content in order to take ownership of all replicas for a set of keys
- pastry requires inherent trust within the network
- routing via bits: always try to route to another node with more bits
- locality-based routing
    - want to route towards destination, but try to stay within a node that is
      "close" (latency-wise)
    - lower latency in getting closer to node
    - DHT potentially very slow compared to a database
- number of hops not always necessarily correlated to routing distances
    - circuit switches between packet switching routers


## Lecture 16

- Data center networks...very different from the internet
    - everything in a warehouse
    - easier to upgrade
    - fully administered
- DC network requirements
    - scalability: incrementally add/build new hosts
    - reliability: loop-free forwarding
    - VM migration
    - reasonable management burden...humans? they can manage
      downtime/misbehaving hardware
- DC management: L2 vs L3
    - ethernet switching: (layer 2)
        - cheaper switch equipment
        - fixed addresses/auto-configuration
        - seamless mobility, migration, or failover
    - IP routing (layer 3)
        - scalability with hierarchical addressing
        - efficiency through shortest path routing
        - multipath routing through equal-cost multipath
    - datacenters often connect layer-2 islands by IP routers
- ethernet
    - flat addressing + self-learning w/ autoconfiguration
        - "plug and play"
    - some monitoring apps require server with same role to be on the same VLAN
    - use same IP on dual-homed servers
    - VM migration simpler
- traditional DC topology
    - L3 router typically at edge of DC-Internet
    - base servers connected by L2 ToR switches
    - aggregation layer with L2/L3 switches to allow inter-server communication
- ethernet scaling issues...
    - flood-based delivery: frames to unknown destinations are flooded
    - bootstrapping on network relies on broadcasting
        - vulnerable to resource exhaustion attacks
    - inefficient forwarding paths
        - loops are fatal (broadcast storms)
        - forwarding along a single tree is inefficient, and leads to
          lower utilization
- traditional topologies
    - over subscription of links higher in the topology
    - tradeoff between cost+provisioning
    - SPOF
- FAT tree-based topology
    - ALL L3 solution
    -  connect hosts with "fat trees"
        - infrastructure consist of cheap devices
        - each port supports same speed as end host
        - all devices can transmit at line speed if packets are distributed along existing paths
        - k-port fat tree can support k^3/4 hosts
    - FAT tree challenges
        - L3 only use one of existing equal cost paths
        - packet re-ordering occurs if L3 blindly takes advantage of
          path diversity


## Lecture 17

- At the time...
    - datacenters still in their infancy
    - at the time, not many buildings with tons of computers
        - supercomputers
        - enterprise/offices?
    - miranet -- originally for HPC, now applied to commodity hardware
- previous network had a bandwidth issue:
    - single-rooted tree all needs to go through root -- the uplink
      bandwidth from layer 2 to 1 must be equivalent to the aggregate
      bandwidth of all servers
- two-level routing table available on commodity switches or not?
    - highest-level prefix matching available on existing routers
        - lowest prefix match on 2nd table maybe implemented with ECMP?
        - ECMP: statically allocates flows of traffic from/to
          source/destination to a path
        - problem with this new architecture is the need for dynamic
          path changing depending on large flows


## Lecture 18

- fb DC workload...not necessarily the same as what fat-tree recommended
- facebook workloads --> not a cloud provider. very specific workloads

## Lecture 19

- software defined network...
- what is it??
    - control plane of network physically separate from the data plane
    - single logically centralized control plane controls the forwarding devices
- networking planes...
    - data plane: processing and delivery of packets with local forwarding state
        - forwarding state + header --> forwarding decision
        - filtering, buffering, scheduling
    - control plane: computing the forwarding state in routers
        - determine how/where packets are forwarded
        - routing, traffic engineering, failure detection/recovery
    - management plane: configuring and tuning the network
        - traffic engineering, ACL config, device provisioning
- control plane
    - compute paths the packets will follow
    - populates forwarding tables
    - traditionally..distributed protocol
    - e.g. Link-state routing (OSPF/IS-IS)
        - flood entire topology
        - each node computes shortest paths --> dijkstra's
- inter-domain routing
    - inter-domain routing, BGP artificially constrains routes
        - routing only on destination IP blocks
        - can only influence immediate neighbors
        - difficult to incorporate other information
    - application-specific peering (route video traffic one way, http another)
    - blocking DoS --> drop unwanted traffic upstream

## Lecture 20

- before active networking..everything baked into the switch and logic
  needs to be pre-programmed
- active networks allow applications to decide how to route packets
- if code is included in the packet, then metadata may not be necessary
    - ip metadata somewhat redundant
- to support in-packet instructions
    - routers would need to have an available instruction set
    - need safeguards
    - router needs its own OS
- research didn't "have a home" yet..kind of died
- applications of active networking
    - bigger issues (e.g. firewalls) is not well-suited for current
    - distributed systems ... hard to manage without active networking
        - application developers lose control
- active networking, not a good home for global internet
    - security issues
    - management issues
- who would benefit from tight network management?
    - different types: small networks (e.g. doctor's office), government, ISPs,
      datacenter/cloud providers. Which ones?
    - government (classified / info compartmentalization)
    - banks/finance companies


## Lecture 21

- ethane-- great experiment in actual deployment
- switch had to be built from an FPGA

## Lecture 22

- congestion control overview
    - challenge: efficiently share network resources?
    - today: TCP
    - Alternate solutions:
        - fair queueing, RED, Vegas, packet fair, rate control, credits
    - no one "true" solution to congestion control
    - too much congestion ==> congestion collapse
- depends on assumptions and goals:
    - hosts, links
    - fairness
    - backcompat
- ACK pacing in TCP
    - ACKS open slots in congestion/advertised window
        - bottlenecking link determines the rate than can be sent
        - ACK indicates a packet has left the network
- problems with ack pacing
    - ack compression
        - vriations in queueing delays on return path change spacing between acks
        - worse with bursty cross traffic
    - what happens after a timeout
        - potentially no acks to time packet transmissions
    - congestion avoidance
        - slow start to last successful rate
        - back to AIMD
- TCP achieves fairness at the cost of efficiency
- TCP friendliness
    - problem: many different TCP implementations
    - if implementations change rate differently depending on drops, more
      aggressive implementations will take more bandwidth (i.e. intertwined
      MIMD/AIMD)
    - incentive to cause congestion collapse
        - if you are more aggressive grabbing bandwidth, you can have more network
        - ...kind of game theory-like

## Lecture 23

- more congestion control...
- high bandwidth-delay product networks
    - high latency links that can have a lot of packets in flight
    - can have lots of delay, bandwidth, or both
- examples of high BW-D product network
    - satellite links: latency 100s of ms to 1s links
- how does XCP deal with the staleness of congestion information in high BWDP
  networks?
    - XCP gives constant flow of congestion information to receivers
    - senders get updates every 1 RTT


## Lecture 24

- congestion control....again!
- trend in adding buffers (memory) into switches to buffer large traffic flows
    - this is always good..right? wrong!
    - hard to determine link bandwidth via loss-based congestion control unless
      the queues are full
    - senders can't mitigate or change rates until the buffers in the network
      are full
- implications of congestion control for large company: lots more money because
  additional need to retransmit packets.
- when sending faster than BW-D product...:
    - at BWDP you are optimally using the network
    - above BWDP -> start queueing
- getting rid of queues could lead to easily measuring BWDP
    - but, if you get rid of them, then we'll likely have much higher packet
      loss rates when bursts do come
- BBR is still loss-based congestion control, but doesn't require filling up
  queues in the network to change transmission rates
- can't simply measure bandwidth at the receiver side
    - bbr measurement does not give the exact measurement of the link bandwidth
    - measures bandwidth subtracted from what others are using at that time.
- bbr doesn't consider fairness of other users of the links
    - bbr at the same time as older versions of TCP, bbr will win over other
      congestion control algorithms
- 