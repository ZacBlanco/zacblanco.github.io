## Introduction

- coordinate processes of distributed applications
- guarantee of FIFO execution per-client
- expose an API that lets developers create their own primitives
  - _coordination kernel_
- two important properties
  - FIFO client ordering
  - linearizable writes
- use "watches" to "subscribe" to updates on particular objects


## ZooKeeper Service

- Abstract all data as _znodes_
- filesystem hierarchy
  - exploit to split different applications which may user zK
  - store small amounts of metadata info
- _ephemeral_ and _regular_ znodes
  - only _regular_ znodes may have children
- linearizability is actually _A-linearizability_
  - (Asynchronous)
- primitives implementable on ZK
  - configuration management
  - rendezvous
  - group membership
  - simple locks


## ZK Implementation

- assume faulty servers
- metadata tree held in-memory
- broadcast updates to all ZK nodes through Zab, an atomic broadcast protocol

> Zab provides stronger order  guarantees  than  regular  atomic  broadcast.   More specifically, Zab guarantees that changes broadcast by a leader are delivered in the order they were sent and all changes from previous leaders are delivered to an established leader before it broadcasts its own changes

- use a "fuzzy" snapshot which doesn't lock state while taking checkpoint
- can do this because state changes are idempotent
- timeouts to detect client failures
- 