---
layout: page
---

## Introduction

- datacenter networking is changing computing and network engineering
- not many validated architectures
- currently available ones come from web search (google?), not necessarily
  representative
- best architecture depends on communication patterns
- DCs are typically oversubscribed on the network
- generic findings
    - traffic is neither rack-local nor all-to-all. Generally low utilization
    - demand is wide-spread, uniform, and stable with rapidly changing
      internally burst heavy loads
    - aside from Hadoop, small packets, continuous arrivals, many concurrent
      flows
- most different architecture proposals (e.g., circuit-like or hybrid) require
  predicting traffic patterns
- most proposals cannot be evaluated without significant investment
    - testing usually simulated or at a small scale
- all previous large studies consider microsoft datacenters
- data collected from FB-wide monitoring systems and packet-header traces,
  examine services generating the majority of FB traffic


## FB Datacenter

- layout
    - many sites
    - 1+ datacenters per site
    - multiple clusters per datacenter
- machines in racks and connected to ToR switch (RSW), 10gbps
- each RSW connected via 10gbps to 4 aggregation/cluster switches (CSW)
- all racks served by a particular set of CSW is a cluster
- aggregation switches connected to another layer called "Fat Cats" (FC)
- CSW connected for intra-cluster routing + inter-DC routing, and some routers
- each machine has precisely one role (DB, web server, etc)

### Constituent services

- web servers, caches, DB to handle web requests
- hadoop clusters in separate racks (HDFS, MapReduce)
- two data sources
    - fbflow
        - production monitoring system sample packet headers
        - 1:30k sample rate
        - data logged, scribe tags and passes data to scuba analytics system
    - fbflow sampling-based collection prohibits some analysis
        - statistics only per-minute
        - for a single server in a rack, turn on port mirroring for full traffic
          collection.
        - special kernel module for buffering incoming packets and sent to
          remote storage
        - limited to only a few minutes of traces

## Provisioning

- hadoop traffic matches literature, but not other services (web server, db,
  etc)
- generally, 99% of links < 10% loaded
- hadoop clusters use more bw than frontend
- hadoop is intra-rack and 80% of traffic intra cluster
- frontend is lighter, but minimally rack-local
- diurnal patterns only affect amount, but not traffic destination patterns
- more traffic patterns
    - lots of rack-local communication with hadoop
    - rack-to-rack communication on front-end cluster varies a lot. lots of
      inter-rack communication, amount depends on service
    - cluster-cluster communication seemingly random.

## Traffic Engineering

- flow stats
    - flow size
        - web server, mostly < 100kiB, all traffic types generally max around 1MiB
        - cache follower: intra-rack/inter-DC traffic < 10KiB, intra-cluster/DC ~1MiB
        - hadoop: most traffic is < 1KiB, intra-rack is ~100KiB
    - flow timing:
        - web server: flows gradually up to about 10k ms, nothing really past 10k ms
        - cache: flows generally flat until 10k ms, the rest 100k ms
        - hadoop: intra-DC flows jump at ~500ms, then flat and peak at 10k ms.
          inter-DC/intra-rack 70% < 10ms, intra-cluster 50% < 10k ms, rest < 100k ms
- heavy hitters
    - persistence of flows making up 50% or more of traffic is low, accounting for no more than 15% of flows
    - considering rack-level flows heavy hitters become more persistent into the
      100ms range for web servers.
    -

## Switching

- hadoop: 60% of packets 1500 (MTU) or TCP acks
- the rest: most packets are < 400 bytes
- packet arrivals: traffic generally similar, 25% around 1ms, 50% at 5ms, 100% ~100ms
- switch buffers have 2/3 utilization even though switch is < 1%
- max buffer occupancy in web server approaches the configured limit for 75% of
  24 hours period
- concurrent flows: web servers have 100s to 1000s of concurrent connections
- hadoop only about 25 cnxns
- most flows traverse an uplink port - consider flow at rack level
- cache: communicate 225-300 racks
- web servers communicate with 10-125 racks
-
