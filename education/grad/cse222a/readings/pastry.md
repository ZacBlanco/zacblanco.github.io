---
layout: page
---

## Pastry

- big p2p lookup system
- routing is one of the hardest challenges
- each node has an ID, and when presented with a message, pastry
  efficiently routes the messages to the closest to that node
- takes into account network locality
    - minimize routing hops
- SCRIBE - pub/sub built on pastry
- PAST - storage built on pastry

### Design

- self-organizing overlay network
- each node routes requests and interacts with local application instances
- nodes with adjacent node IDs may be divers in geography, ownership,
  network, etc.
- route to given node in $$log_{2^b}(N)$$ hops
- routing guaranteed unless L/@ nodes with adjacent nodeIds fail simultaneously
- pastry node state
    - each node has a routing table, neighborhood set, and leaf set
    - routing table has $$log_{2^b}(N)$$ rows
    - $$2^b - 1$$ entries per row
    - the entries at row $$n$$ of the table refer to a node whose nodeId shares
      the present node's nodeId in the first $$n$$ digits.
    - the neighborhood set contains nodeIds and IP addresses of M nodes closest
      to the local node
        - neighborhood set not normally used in routing - useful for maintaining
          locality properties
        - leaf set used for routing
- routing
    - when a message arrives
    - check if key falls within range of nodeIds covered by the leaf set
        - if within range, message is forwarded directly to the destination node
    - if the key is not covered by the leaf set, the routing table is used and
      the message is forwarded to a node that shared a common prefix by at least
      one additional digit
    - in the case where the routing table is empty or the associated node is not
      reachable, then forward the message to a node that shares a prefix with
      the key at minimum as long as the local node **and** is numerically closer to the key than the present node's ID.
    - in the case of many simultaneous node failures, number of routing steps
      required is at worst linear in N.
    - API
        - nodeId = pastryInit(credentials, app)
        - route(msg, key)
        - deliver(msg, key)
        - forward(msg, key, nextId)
        - newLeafs(leafSet)

- self-organization
    -
