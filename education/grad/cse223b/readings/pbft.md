---
layout: page
---

## Introduction

- replication algorithm to tolerate byzantine faults
- "practical" algorithm for state machine replication
- liveness and safety with less than `(n-1)/3` machines faulty
- other systems rely on synchrony for correctness
  - synchrony is supposedly "not practical"


- System Model
  - assume faulty nodes behave arbitrarily
  - cryptographic techniques to prevent spoofing, replays, detect corrupt messages
  - assume that an adversary can coordinate faulty nodes, delay communications
- Service Properties
  - generic - implement deterministic service with state and operations
  - replicated system satisfies linearizability and behaves like centralized impl
  - does not handle leaking of informatino from faulty replica


## Algorithm

- basic
  - client sends request to invoke operation to primary
  - primary multicasts requests to backups
  - replicas execute request, sending reply to client
  - client waits for f+1 replicas with the same result
- replicas must be deterministic
- additional replicas will degrade performance
- one primary, multiple backups -- assignment of roles at particular
  time is a _view_

- replicas contain state of service, and message log with messages of replica
- upon receipt, primary initiates 3-phase protocol for atomix multicast
  - phases: pre-prepare, prepare, commit
- pre-prepare:
  - assign sequence number, multicast pre-pare with request
- prepare
  - upon verified receipt of pre-prepare, move to prepare phase
  - send prepare message to all other replicas adding both to logs
    - `prepared(m, v, n, i)` predicate
      - `TRUE` if got prepare message m in view v, SN i, and received 2f
        prepares from different backups
  - when `prepared` is true replica multicasts a commit message to other
    replicas for `m`
- remove messages from log at some specified sequence number interval
  - "checkpoint"
- view changes
  - view changes triggered by timeouts
  - timer between requests
    - stop timer if no requests
  - multicast view change with new view ID
  - multicast new view message after receiving 2f valid view changes for
    v+1 from other replicas
