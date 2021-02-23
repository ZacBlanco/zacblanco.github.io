## Introduction

- expertise with clusters of commodity PCs have enabled harnessing and
  petaflops of computational power
- principle bottleneck is network
- applications with cluster-based FS require remote-node access
- can connect with a special fabric like infiniband (not cheap)
    - also not commodity
- commodity hardware tough to get proper communication bandwidth
- goals:
    - scalable interconnection bandwidth
    - economies of scale
    - backwards compatibility
- fat-tree architecture


## Background

- common interconnect is hierarchical, ToR with aggregation routers,
  core routers at top
- less nodes in higher layers
- 2-tier can have 5-8k hosts
- 25k (target) requires minimum three tiers
- switch types:
    - 48 port 10GE ToR/edge
    - aggregate/core -- 128-port 10GE
- oversubscription: worst-case achievable aggregate bandwidth among end hosts to
  total bisection of bandwidth of a particular communication topology
- full bandwidth require multi-rooted tree with multiple core switches
- cost: $7k per 48-port, $700k per 128-port
- aggregate arch: 20k hosts: $37M
- fat:tree: $8.64M

### Clos Network/Fat-Tree

- use pods of switches responsible for each rack
- use path redundancy to increase bandwidth. Each host connected to pod of
  switches
- pod of switches can route among any of the core routers
- use two-level routing tables
    - distribute traffic among switches
    - first table may be "terminating" if there are no second-level suffixes for
      the entry in the first table
    - first table based on prefix masking, 2nd table on suffix matching
    - no more than k/2 prefixes and k/2 suffixes per switch
- routing table lookup latency
    - use TCAM (Ternary Content-Addressable Memory), lookups in hardware
- routing algorithm
    - packets to particular destination always follow the same path
    - packets to hosts in a different pod may take different paths depending on
    the destination
- flow classification: dynamic routing supported in most enterprise switches
    - pod switches need to recognize packets of same flow, and forward them on
      the same port
    - periodically reassign small number of flow outputs ports to minimize
      disparities
- flow scheduling
    - most internet traffic characterized by few large flows, the rest is very
      short and bursty.
    - consider flow scheduling of long-lived flows to improve overall
      performance
    - initially assign flow to least-loaded port
    - use central scheduler tracks active large flows and assign non-conflicting
      paths
- fault tolerance
    - many redundant paths in fat tree - can handle failures
    - broadcast protocol to allow switches to route around link or switch
      failures
    - link failure between lower and upper level:
        - outgoing inter and intra-pod traffic originating from low switch: cost of link is now infinity
        - intrapod traffic using upper using upper=layer switch as intermediary: in failed link, broadcast a tag notifying lower-layer switches in the same pod of the link failure
        - inter-pod traffic coming into upper-layer: upper-layer broadcasts tag to all upper-layer switches connected to in other pods.
    - failure of upper-layer to core switches
        - outgoing inter-pod traffic: local routing table marks affected link as
          unavailable, chooses another core router
        - incoming inter-pod traffic: switch broadcasts tag to all other upper-layer switches directly connected to it.
- power consumption is less than half as much as previous architecture
- downside: packaging -- lots more cables are required for the interconnection of all machines
