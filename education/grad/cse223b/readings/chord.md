---
layout: page
---

## Introduction

- distributed key-node mapping lookup service
- uses consistent hashing for log(N) lookups
- each node need only information about log(N) neighbors

## System Model

- addresses:
    - load balancing
    - decentralization
    - scalability
    - availability
    - flexible naming
- potentially useful applications:
    - cooperative mirroring
    - time-shared storage
    - distributed indexes
    - large-scale combinatorial search

## Chord protocol

- assume symmetric and transitive routing between nodes
- maps keys to node responsible for the key
- improves on scalability of consistent hashing because instead of every node
  needing to know about every other node, it only needs to be aware of log(N)
  nodes.
- hash node IP to get node identifier
- identifiers exist in a ring from 0 to 2^m
- key belongs to a node who is the successor of the key hash
    - if key is > next node - first node clockwise from k
- all keys of a node when leaving are assigned to N's successor
- when a node joins, all keys between the new node's predecessor and the new
  node are migrated to the new node
- need a good hashing algorithm which evenly distributes the data
- Simple lookup algorithm
    - linear O(N) by knowing only the successor
- scalable lookup algorithm:
    - m entries in the _finger table_
    - ith entry in the table at node n contains the identity of the first node succeeding n by $$2^{i-1}$$ keys
    - lookup by first checking current node, then locating closest preceeding node in finger table
- dynamic operation and failure
    - all nodes must have up-to-date successor information
        - only 1 node needs updating upon a join
    - periodically `stabilize` to update successors
    - periodically `fix_fingers` to update finger tables

- Theorems
    - 4.3: if any sequence of joins is executed with `stabilize`, some time
      after the lat join successor pointers will form a cycle on all nodes in
      the network.
    - 4.4: After N nodes join a system of existing N nodes, lookups still occur
      with log(N) with high probability

- failure and replication:
    - correctness relies on ever node knowing its successor. If a node fails,
      this may be compromised
    - each node maintains a successor list of its closest `r` successors`


