---
layout: page
title: CSE 223B - Distributed Systems
permalink: /education/grad/cse223b/
keywords: distributed, systems, CSE, 223B, UCSD, UC, San Diego, networking, tcp, udp, ip, software, operating, system, linux, C, rust
description: Notes for CSE 223B (Distributed Systems) at UC San Diego
mathjax: true
notes:
  bayou: Bayou
  chord: Chord
  ivy: Distributed Shared Memory
  farm: FaRM
  fawn: FAWN
  frangipani: Frangipani
  harp: Harp
  isis: ISIS
  pbft: Practical Byzantine Fault Tolerance
  treadmarks: TreadMarks
  zookeeper: ZooKeeper
---



## Paper Notes

{%for pg in page.notes -%}
- [{{ pg[1] }}](./readings/{{pg[0]}}.html)
{% endfor %}



## Lecture 1

- types of distributed systems:
    - P2P: route requests to one or more equivalent peers
    - Main frames w/ dumb terminals
    - datacenter: high speed connections within a particular locations, many clients connect
- why even build a distributed system?
    - scalability
    - reliability/robustness
    - legal/policy
    - performance/speed of light
- issues with distributed systems
    - heterogeneity
    - autonomy
    - transparency - do users see the distributed aspecT?
    - coherency - are users affected by others who use the system?


## Lecture 2 - IVY - Distributed Shared Memory

- distributed shared memory can't have the same semantics as traditional single-
  machine multiprocessing environment

Example:

```
MUL r1, r2, r3
ST x1, r1
LD y1, r4
```

- in a real system, we pipeline instructions and this doesn't _really_ work
    - MUL, ST, and LD can all take multiple cycles
    - LD can be executed before or after ST if x1 != y1

- consider two threads
    - cpu0: `value = compute(); done = true`
    - cpu1: `while(!done); use(value)`
    - generally compiled into two stores, and two loads
    - no dependencies between instructions, does this code even work?
    - on x86 it works, but other processors it may not work
- need some kind of rules in how processors are designed, _consistency models_

- strict consistency models
  - each CPU executed its instructions in order
  - each LD instruction gets the value of the most recent ST to that
      address
  - **strict consistency**
  - with a sufficiently precise wall clock, this is well-defined
- relaxed consistency model
  - each CPU executes its own instructions in order
  - all CPUs see all instructions in the same _total_ order
    - instruction interleaving is arbitrary
  - **sequential consistency**

- how to implement sequential consistency?
  - cpus need to agree on a total order
    - "no CPU needs to actually know the total order"
- "all operations on a memory location are ordered by that location"
  - typically a serialization point in the system which picks the total order
  - in IVY, locks on page data structure creates the serialization

- Situations distributed memory system (IVY) must handle:
  1. read page it already has
  2. read a page it doesn't have
  3. write a page it _owns_
  4. write a page it already has and does _not own_
  5. write a page it _doesn't_ have

- optimize for 1, 3, then the ordering may change depending on what you design
  the system to be good at.


## Lecture 3 - IVY cont'd

- Five cases from IVY
  - read a page which is owned or have cached
    - nothing!
  - read a page I don't have
    - ask manager - go to owner
  - write a page I own
    - no cached copies valid anywhere, not much else
  - write a page I have cached (read-only)
    - need to invalidate all pages in the copyset
  - write a page I don't have
    - need to go to the manager for a copy
    - require acknowledgements for all messages at manager before
      releasing the lock on page table

- Improvements & optimizations
  - change location of copyset: owner handles copyset instead of manager


- Treadmarks
  - ivy didn't have multiple copies of pages - processor needed to  collect/delete all out of date versions
  - system benefits writes instead of reads
  - release consistency
    - all stores are visible everywhere at release
  - lazy release consistency
    - only needs to be visible at the next acquire


## Lecture 4 - TreadMarks + Chord

- Treadmarks release consistency
- acquire and release of a lock both increase the vector clock


```
cpu 0: a(l); x = 1; r(l); ...
cpu 1  ...                | a(l); y = x; r(l); ...
cpu 2  ...                ...                  | a(l) print x, y; r(l) ...
```

- all start at vector clock `[0, 0, 0]`.
- upon release, TM sends a write notice with the vector clock
  - cpu0 sends its clock as `[2, 0, 0]`
  - cpu1 sends its clock as `[2, 2, 0]`
  - cpu2 sends its clock as `[2, 2, 2]`
- When grabbing page for a page fault, the TM client will ask for the page diff from any host who has a larger VC than the last update for the page.

New example with additional lock:

```
cpu 0: a(l); x = 1; r(l); a(l2) z = 99; r(l2); ...
cpu 1  ...                | a(l); y = x; r(l); ...
cpu 2  ...                ...                  | a(l) print x, y; r(l) ...
```

- in this case, cpu2 would see two different timestamps from cpu0 and cpu1 which are incomparable:
  - cpu0 has `[3, 0, 0]`
  - cpu1 has `[2, 2, 0]`


- Chord
- System of nodes in a system which can join and leave at will.
- similar to associative memory

- rules
  - all operations happen in order
  - sequential consistency
- consistent hashing -- map everything to a consistent space
- new node always needs to know about the successor (based on consistent
  hash)
- as long as the successor is correct, everything should work...

## Lecture 5 - Distributed Filesystems (Logging)


- harp main goal: fault tolerance for system failures
- fault tolerance technique: primary-copy replication
- all operations are serialized at the primary
  - able to achieve a total ordering among file operations
- assume "fail stop"
  - failure defined by all work completely stopping
- client needs to know when data is safe
  - the "commit point" is safe to tell the client that its operation has
    persisted
  - safest commit point is when item is persisted on disk of copy (not
    how harp works)
- availability:
  - if primary crashes and we don't promote a backup --> no work gets
    done
  - network partition --> possible split-brain
- need a "quorum" to continue processing requests
- witnesses joins with old primary or old backup to try to bring one
  group to a quorum
- witness durably logs operations while it is promoted
- harp needs to be able to survive failures with quorums
  - multiple failures in a row --> each quorum over time is assigned a view
  - new primary may be elected for each view


- each node has its own lower bound and commit point
  - commit at primary vs backups: primary is the serialization point
  - backup needs to know what is safe to write to disk (can't pass
    primary commit point)
- writes to disk in batches (apply point)
  - cannot pass the commit point
  - difference between apply point and lower bound are in-flight to HDD

## Lecture 6 - Distributed Filesystems (Metadata)

- harp gives stronger consistency than necessary
- frangipani goal: scalability or _bust_
  - add nodes: clients only increase CPU
  - add disks to increase storage

- scaling depends on how well lock service scales
- someone else interested in inode on path to file you're interested in
  - unix FS has "last file wins"
- synchronous metadata updates on FS metadata (create file, dir, rm file/dir)
- what locks are required for a metadata update?
  - just write locks on the inodes
- data blocks in a file, locks are not required
  - blocks covered by inode locks
- every server gets their own free block list to prevent single serialization
point.
- each server has its own inode blocks (on top of data blocks)

- metadata logging
  - write TODO entry in log
  - metadata updates
  - check TODO item off in log
  - reply to client

- contention:
  - where does log go??
    - occurs at each file server
      - on private log in petal
  - after writing TODO entry, must acquire locks
    - release after metadata update
  - must find full set of locks, all locks acquired in proper order


## Lecture 7 - 2-Phase Commit

- simpler systems
  - replicated state machines (harp)
  - operations are serialized, resulting in some kind of sequential consistency
- What about longer operations? (transactions)
- bank example
  - two banks, need to transfer money from bank 1 to bank 2
  - payment processor
    - needs to know both banks are willing to perform the transaction
  - bank 1
    - wants the money to move (at most once)
  - bank 2
    - wants the money (at least once)
  - need clear semantic on the transaction
    - each part of the transaction happens exactly once, or nothing happens
      - atomicity
    - tradeoff -- liveness or safety
      - can design a system which is always _one_ but cannot _always_ have both
- transactions need some kind of ordering
  - can't just ack between parties in the transaction
  - use a Transaction Coordinator (TC)
    - middle man between parties issuing the commands to different (banks)
  - use a log of linearized transactions to track state of each party
- two phase commit
  - TC gets request for transaction
  - send "Phase 1" message (are parties willing?)
    - if parties say Yes: in some future period of time in the future, the transaction _will_ occur
    - if parties say No: in some future period of time in the future, the transaction _will not_ occur
  - TC then can send a phase 2 message
    - commit or abort message
    - each party will all get commit, or all will get abort
- if saying no to a transaction, then its safe to abort
  - party 1 can speak to party 2 about prepare message to check that
    other is ready to commit the transaction
- TC is the only party which can commit the transactions
- phase 2: promise to commit/abort the transaction

- presumed abort: always assume transaction is aborted up until the commit is received
- unless TC writes commit for transaction in log, then

## Lecture 8 - Group Communication (ISIS)

- Things we want in group communication
  - group addressing
  - fault tolerance
  - ordering
  - synchronization
  - consensus

- additionally; determine liveliness, safety
- types of message arrival orderings:
  - total
  - causal
  - FIFO
  - unordered
- types of reliability
  - None, reliable, until/unless abort
- UDP: no reliability, unordered
- TCP: reliable, unordered
- ISIS: attempts to give more space
- ISIS protocols:
  - ABCast
    - total ordering, reliable
  - CBCast
    - causal broadcast
    - causal ordering, reliable
  - GBCast
    - causal, total ordering, reliable
- goal of ISIS: performance win, with an ordering only when you care (labels)
- virtual synchrony --> series of views
- types of protocols
  - send message to everyone with ACK
  - all recipients send message to everyone with ACKS
    - if first time you receive the message

- CBCast - causal ordering
  1. cpu sends M_a before M_b then M_a --> M_b
  2. cpu receive M_a before sending M_b then M_a --> M_b
  - implemented w/ vector clocks
  - only communicate with other nodes by sending entire message queue
- ABCast - total ordering
  - must come to agreement on ordering of messages
  - can only consume head of queue if message is marked as deliverable

## Lecture 9 - Paxos 1

Sorry, no notes today `:(`

## Lecture 10 - Paxos 2

- most common operation needed for paxos is the "view change operation"


Paxos phase 1

- Initiator:
  - Any node may start the phase one: _the initiator_
  - set `done = false`
  - `my-n = max(n_n, my-n) + 1` + append node ID
  - send prepare message to all nodes the the current view
- Everyone else:
  - on receive `prepare(view_id, n)`, if `view_id <= view_id_h`, send current view
  - else if the new view id is greater and the proposal number is greater the
    higher proposal number
    - set new n  if the proposal is higher than anything you've ever seen
    - done is false, send prepare with new id
  - else, if the view id is the same, but new id already the same

Paxos Phase 2
  - if receive and old view message with a view ID and N
    - set new highest view to view
    - view change -- restart paxos
  - else if receive a `reject`, delay, then restart paxos
  - else if recv a prepare from the majority of views with the same ID
    - set v = v_i for highest n received
    - if there is no non-empty view, pick the new view
      - send `accept` with view id, my n, and the view
    - else: wait a little and restart paxos
  - if receive an accept with a view ID, n and a new
    - if the view id is < highest seen view id, send oldview response
    - else if the n is greater than the highest proposal we've seen,
      - set the new view and proposal as highest, send `accept`
    - otherwise, `reject()`

Phase 3 (learning phase)

- Initiator
  - if receive an accept from the nodes in a view, then send the `decide`
    message with the view to everyone
- Everybody else
  - if recv a decide with a view

## Lecture 11 - ZooKeeper

- Quorum system which is designed for high performance (unlike previous
  services, like chubby)
- replication layer for availability, liveness,
- clients connected to at most one server

- in ZK, all writes from a single user are in order
- All operations are serviced in a total order

- semantics of ZAB: ABCAST

- What does ZK _actually_ do
  - everything stored in FS format
  - terrible fs performance -- all writes need to run through ZAB
- two interesting ZK features:
  - `SEQUENTIAL` flag for creating filenames with monotonically increasing #'s
  - `EPHEMERAL` flag for znodes which only last for a short period of time
    - if client disconnects, delete all ephemeral files
- for implementing locks, use files!
  - can make use of the watch notifications to prevent the need to spin
    waiting for the file
    - this scales terribly because of the amount of work required scales w/
    the number of acquirers
  - instead of using a single lock file, use the sequential flag to
    create my own lock file, but only acquire lock once "my" file has
    the smallest sequence number in the directory
- in view change, new quorum can't be made until all locks in old quorum
  have expired


## Lecture 12 - FAWN

- main idea of FAWN is to be energy efficient
- datacenter efficiency measured as PUE (power usage effectiveness)
- KV store usage is flipped upside down compared to power
  - uses little CPU
  - a bit more storage, some DRAM
  - want mostly network
  - modern power draw of a server: CPU > DRAM > storage > network
- flash storage
  - virtual aliasing -- load levelling algorithms in drivers/firmware

- chain replication
  - puts/stores all serviced at the head of the chain
  - all reads/gets go to the tail of the chain
  - reads are "committed" once it gets to the tail of the chain

- node joins/leaves
  - all nodes need to agree when a node joins/leaves



## Lecture 13 - FaRM

- networks can be sloooow
- to transfer data over network via RPC
  - rpc protocol calls `send` to trigger tcp stack in kernel
  - syscall (expensive)
  - kernel needs to copy userspace buffer
  - NIC needs to poll/be interrupted to grab kernel buffer, copy to NIC,
    send to other server
  - to copy data in kernel, copy to NIC, etc.. very slow
  - FaRM argument: can't make use of all BW with traditional TCP stack
    - use RDMA (remote direct memory access)!
- RDMA, skips CPU copying to NIC
- RDMA one-sided operation (RoCE)


- FaRM: use large, fast, DSM system
  - use 2PC
- _optimistic concurrency control_
  - use `txBegin` to initiate transaction, more like a hint than a hard lock
    - system will check after `txEnd` if the transaction was actually atomic
- every piece of data has a version number
- assumes that all writes happen in increasing order
- version stored at the end of the block
  - lock at front of data header
  - b/c data doesn't get read in increasing order, then each cache line needs
  a version number.
- problems with version on cacheline:
  - trade-off between space overhead of storing the version, and likelihood
  of overflow (used 15 bits)
  - version checks must be timely to prevent overflow

## Lecture 14 - Byzantine Fault Tolerance

- previous discussion all assumed fail-stop behavior
  - upon a failure, process crashes, no more messages
- fail-stop is not necessarily how things work
- potentially there are _byzantine_ failures in which a node is faulty
  and sends false messages, or doesn't behave in the expected manner
  - just because node is live, doesn't mean it is sending "real" messages
- must have 2f+1 honest nodes in the system in order to successfully
- for view changes, need f+1 nodes to agree on the view change
- general operation
  - client ops contains (timestamp, message)
  - client -> primary: send request with (0, t, c)
  - primary -> replica: pre-prepare w/ (view, sequence num, d)
  - backup -> replica:  prepare w/ (view, sequence num, d)
  - primary -> replica commit with (v, n, d, i)
  -  upon receiving f+1 commits, state predicate is `commit-local`
- client must receive at least f+1 responses

## Lecture 15 - Optimistic Consistency

- "disconnected operation"
- built on AFS (Andrew File System)
- AFS uses a local cache on client machine
- AFS uses whole file caching
- normal fs operation:
  - writes don't happen immediately on the `write` syscall
  - only guarantee writes on `flush` or `close`
- pessimistic consistency -- virtually unusable
- optimistic consistency -- make the assumption that most people are not
  going to write the same file(s)
- don't want everyone to "lose" when 3 groups can read/write files in
  the face of disconnected users (disconnected user, connected user(s),
  everyone)
- coda: no locks
  - callback notifications -->  invalidations
- compare file versions between
- LSID --> `<server ID, sequence num>`
- coda clients hoard inodes/free blocks
- use hoard profile to determine which files should be around
  - buffer cache contents --> heuristic of hoard profile + LRU
- hoard walk
  - determine if any hoard files need to be downloaded
  - will only remove things which the hoard profiles outrank
  - need to also cache entire tree (of directories) above

## Lecture 16 - Bayou

- ..... rewatch first 40 mins of lecture
- merge procedure for when gossipping about message in converging to a
  total order
  - additional _general predicate_ to determine that a conflict exists
    in the first place
  - in case a possible failure of the merge procedure...write to an error log
    - not ideal, but at least "succeeds"
- most operations in previous papers dealt with redo logs -- an ability
  to apply operations
- in bayou, we need _undo_ logs in order to undo previously added operations
  - for undo logs, you can have tentative operations which need to be _undone_
- garbage collection in bayou
  - nodes remove log at some point in order to not hoard log entries and
    save space
  - if a new node joins, the node which garbage collected its log may
    not be able to catch up the new server if their last commited
    sequence number is not in their log

