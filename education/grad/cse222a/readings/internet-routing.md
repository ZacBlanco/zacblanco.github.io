---
layout: page
---


## How Internet Routing Works

- Chapters 3.4 and 4.1 of the [Systems Approach][https://book.systemsapproach.org]

### 3.4 - Internetworking - Routing

- In circuit-switched networks, routing is only a problem for the first packet
  of the network.
    - all subsequent packets follow the initial circuit path
- datagram networks, every packet must be routed
    - switch can do simple routing by looking at a forwarding table
    - how do switched and routers acquire information in a forwarding table?

- routing is the process by which a forwarding table is built
- forwarding is the process of taking a packet and looking up its destination in
  the forwarding table.

- row in a forwarding table contains mapping from a network prefix to an
  outgoing interface and some MAC information like ethernet address of the next
  hop.
- routing table is built by routing algorithms as a precursor to building the
  forwarding table
    - contains information about how some information was learned.

- most protocols described here are for smaller networks (intradomain or
  interior gateway protocols [IGP])


- **domain**: a set of routers all under the same administrative control


### Distance-Vector Routing (RIP)

- Each node constructs an array (vector) containing the costs to all other nodes.
- Each node distributes its table to the immediate neighbors
- assume each node knows the link cost between each directly connected neighbor
- down link --> infinite cost

- send information to immediate neighbors in a number of rounds
    - update costs appropriately
    - only requires a few rounds for all nodes to gather the required
      information

- updates can be sent periodically
    - can also be sent on link cost change (increase/decrease)
- issues w/ algorithm
    - count to infinity problem
        - solve w/ split horizon (don't send routing information learned from a
          particular node back to that node.)
            - only works with small loops (2 or less)

#### Link State (OSPF)

- Disseminate entire graph to all nodes (over time)
    - each node can build a complete map of the network
    - calculate routes based on link-state knowledge
- _reliable flooding_
    - make sure all nodes receive a copy of information that participate in the
      routing protocol
    - link state packet w/ direct neighbors including cost, sequence number, TTL
- once every node has the information, can use dijkstra's to calculate the
  shortest path.
- algorithm converges quickly and responds to link updates in a stable manner
- downside is the amount of information required to be stored at each node

- OSPF (Open Shortest Path First) Protocol
    - adds authentication
    - from IETF
    - Adds hierarchy to partition domains
    - load balancing of routes


#### Link Metrics

- Measurements of
    - latency
    - bandwidth
    - queue length/time
- did not work very well for different link types in ARPANet
    - once a link too heavily loaded
- how to do this kind of testing?


### 4.1 - Global Internet

- Graph of coupled organizations united by regional providers under the NSFNet
  backbone
- internet itself has a structure of ASes (autonomous systems)
- AS as different areas
- **area** is a set of routers administratively configured to to exchange link
  state information with one another
-  Area border routers (ABR) are routers on the edges of areas
- areas have fully consistent link state information of one another

#### Interdomain Routing (BGP)

- Autonomous system provide a way to hierarchically aggregate information of a
  large internet
- routing problem now simply between AS and within the AS
- Border-Gateway Protocol
    - no assumptions about how ASes are interconnected
    - shortest path a concern, but not most important
    - need to find _some_ path which is **loop-free**
    - even with CIDR, some routers still need to hold about 700k
      prefixes of information (mid 2018)
    - impossible meaningful path cost between ASes (different metrics)
    - AS numbers must be unique (assigned by an authority body) to prevent
      routing loops
    - Advertise information about paths to other ASes with full route info (to
      prevent loops)
    - BGP allows cancelling of routes (downed links, etc) with withdrawn
      messages
    - BGP runs on top of TCP
- common AS relationships
    - provider-customer
    - customer-provider (don't carry other provider's traffic)
    - peer
- eBGP/iBGP for routers within AS to learn which external routers to send traffic to
    - iBGP for internal routers
    - eBGP for external
