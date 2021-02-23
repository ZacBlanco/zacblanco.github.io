

## Introduction

-  present new measurement technique to infer high-quality ISP map while
   using as few measurements as possible
    - directed probing (pick important routes)
    - path reduction (suppress redundant routes)
    - alias resolution problem (clustering IP addresses to corresponding routers)
- 3 ISPs helped validate maps

## Problem & Approach

- ISP network composed of POPs (points of presence)
    - POP is physical location with a number of routers
- ISP backbone connects the POPs
    - routers within the backbone are _core_ routers
    - _access_ routers sit at edge of ISP and neighboring networks
- discover ISP maps using traceroutes
- single map with tr/1.5 s would take 125 days to complete
- choose traceroutes that contribute the most information to a map and omit likely redundancies
    - expected routing paths provide a means to selection
- after collecting traceroutes, difficulties remain
    - TR has list of IPs representing router interfaces: IPs must be resolved to a particular alias
    - identify geographical location of a router and role in topology
    (use location hints embedded in DNS names to extract backbone/POPs)


## Mapping Techniques

- Selecting measurements
    - directed probing using BGP tables
        - BGP tables not available directly, use "RouteViews" which gives BGP
          routing tables from ~60 vantage points
        - **dependent prefixes**: prefix originates within ISP
        - **insiders**: traceroute within dependent prefix **insider**
        - **up/down trace**: likely to transit ISP based on AS path
    - path reductions
        - ingress reduction, egress reduction, next-hop AS reduction
    - alias resolution
        - determine which IP addresses belong to the same router
        - basic: two aliases should respond with the same source address when sending a UDP packet to an unbound port
            - add TTL comparison to aid in check
        - also try ICMP rate limiting technique
        - in-order IP identifiers on split packets used to determine if a single router
    - router identification
        - rely on DNS names as accurate characterization of router owner
        - many routers operate on similar architecture



## Evaluation

- validates with ISPs
    - of 3 ISPs, no POPs missed, one misplaced router due to missing city code
    - miss any links?
        - a few extra links
    - which fraction of routers missed?
        - one with no obvious misses
        - all backbones present, but some access routers missing, 3rd said all router in AS
    - what fraction of customer routers missed?
        - not verified/verifiable
    - ISP rates maps "good", "v good", "v good/excellent"
- generally not as many bgp neighbors identified in map vs routeviews
- beats skitter from caida by a large margin
- impact of reductions
    - directed probing only needs 1-8% of traceroutes requires  by brute force (90-150M)
    - ingress reduction used only 12% or so of traces selected by directed probing
    - egress reduction not universally applicable
        - minimum customer allocation smaller than /24?
    - next-hop AS reduction picks only 5% from directed probing analysis
    - alias resolution
        - 70% of routers have just one IP
        - 10% w/ 2
        - max is 24 IPs on a single router
