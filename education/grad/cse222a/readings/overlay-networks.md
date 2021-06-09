---
layout: page
---

## Overlay networks

- _application processing_ vs _packet forwarding_
- overlay nodes attach "outer headers" to route on the underlying network
- overlay networks don't need to route based on the underlying network headers
    - can have their own headers


### Routing Overlays

- simplest exists is to support alternate routing strategies
- overlay networks _circumvent_ standard IP..allow for new testing


#### Experimental IP

- multicast internet
- used to be a multicast backbone
- MBone used IP tunnels+addresses, forwarding implemented in an new way
- 6-BONE, similar multicast overlay relying on IPv6

#### End System Multicast

- IP Multicast popular with some researchers...still limited deployment
- multicast-based applications like videoconferencing have turned to
  _end system multicast_
- end hosts participating in multicast implement their own multicast trees
- only hosts participate in the tree
- exchange via UDP tunnels
- can be partially solved by using cloud infrastructure with static
  hosts to have a partially static multicast tree where end users
  connect to the closest cloud
- constructing mesh overlay should roughly correspond to the physical
  topology of the underlying internet
- building a mesh
    - get information about neighbors
    - only add links to overlay when finding a new low-latency neighbor
    - nodes send "join mesh"/"leave mesh" messages notifying neighbors


#### Resilient Overlay Networks

- BGP only finds _some_ route -- it may not be the best
- sometimes routing through an intermediate node is faster
- BGP scales to large networks, but does so slowly and does not pick
  optimal paths
- RON showed it can improve performance and could adapt quickly by using
  an overlay
    - not scalable: O(N^2)
- RON in practice --> SD-WAN
- what happens when _everyone_ is using RONs? does it impact net perf?

### P2P Networks

- no server for downloads
- napster...not true p2p
- two processes
    - locate object of interest
    - download object of interest
    - ..how to achieve with no central authority?
        - technically, an overlay network can do this

#### Gnutella

- no centralized registry of objects
- gnutella floods the overlay to search for a particular item
- new nodes join via single link
- learn about other nodes from query response messages
- flooding does not scale


#### Structured Overlays

- form a structure which is reliable and efficient for object location
- challenges: how to map object onto node, how to route request to the
  right node?
- well known technique for mapping names to addresses: hashing
- hashing flaw: how many buckets?
    - also..how to get IP address from hash?
- _consistent hashing_
    - map objects _and_ nodes across a single large ID space
- to consistently place nodes and objects:
    - hash(object) --> object ID
    - hash(nodeIP) --> node ID
- each object maintained on the node which is closest to its hashed ID
    - when leaving/joining, only a small amount of data needs to be moved
- to handle accessing the object for a particular hash: route!

#### BitTorrent

- avoid bottleneck of single file source
- each file shared by an independent network
- swarm nodes join+leave to download the file
- magnet file has file size, piece sizes, SHA, and URL of tracker
- trackerless torrents form a DHT using randomly generated 160-bit IDs
- depends on "good citizenship" of other providing upload/download (seeding)


### CDNs

- four potential internet bottlenecks
    - first mile
    - last mile
    - server itself
    - peering points
- first mile, can't do much about
    - the rest, solve with replication
- akamai -- the original CDN
- cache pages
- only good for static content
- CDNs also need "redirectors" to route requests to best CDN server
- how does redirection happen?
    - 1: DNS
    - 2: request proxy
- redirectors can hash URLs to make requests go to a particular server
  (e.g. serving from the in-memory cache)
-
