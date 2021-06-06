## Introduction

- replicated, weakly consistent storage system
- designed for mobile computing
  - e.g. less-than-great mobile connectivity
- design around supporting app-specific mechanisms to detect and resolve update
  conflicts
- novel methods:
  - conflict detection (dependency checks)
  - per-write conflict-resolution w/ client-provided procedures
- clients may see all writes, even with conflicts
- support potentially disjount groups

## Applications

- meeting room scheduler
  - potentially out of date calendar, many users can schedule for particular
    rooms
- bibliographic database
  - users adding bibliography entries over time, while not connected in the
    library (???)


## System Model

- group of servers to handle reads/writes
- application just needs a write to be accepted by a single server
- writes must include not just data, but information about how to resolve a
  conflict
- clients may interact with multiple servers
- propagate writes to other servers in "anti-entropy" sessions
  - after a session, the two servers will agree on a set of writes

## Conflict Detection and Resolution

- application defines a dependency check and merge procedure
  - dependency check --> execute and if result is not expected, then execute mergeproc
    - if dependency check executes as expected, simple continue with the write
- dependency checks potentially read from replica server
  - can enforce multi-item integrity constraints on data
- merge procedures don't impeded data access


## Consistency

- All server move towards an _eventual consistency_
  - replicas may not contain the same state
- no hard guarantees due to potential network partitions
- writes are _tentative_ when received
  - each has a timestamp
  - writes committed in order of timestamps
- `<timestamp, server id>` pair ensures a total global ordering on all writes

