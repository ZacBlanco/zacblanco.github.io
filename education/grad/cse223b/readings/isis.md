

## Introduction

- communication primitives for supporting distributed computations in
  failure- prone environment
- computation represented as a set of event operating on process state
  with a partial ordering
- premise: event orderings included in communication layer
    - ability for a process to deduce event orderings seen by other processes
    - reduced risk of inconsistent actions
- example:
  - process updating replicated data item maintained by a set of _data managers_
  - using _reliable broadcast_, if any manager receive the update, all will receive it.
  - if broadcast fails the following could occur:
    - manager receives update and detects failure
    - detects failure and receive the update
    - detects failure, and the message is never delivered.
  - can try to resolve by
    - running an _agreement protocol_ (consensus?)
    - discard messages received after learning the sender failed
    - construct a broadcast protocol which orders messages relative to
      failure and recovery events such that problems don't arise


## System Characteristics

- collection of processes, fail gracefully, potentially unreliable network
- no partitioning of network
- protocol needed to agree on failure and recovery events

## Fault-Tolerant Process Groups

- members of group monitor one another
- operations must happen as a group (e.g. consistent view of smallest ID, joins,
  leaves, etc)

- three bcast primitives
  - group broadcast
    - snapshot of membership
    - update knowledge of system only on reception of GBCast message
    - failure gbcast must be delivered after any messages sent by the failed
      process
  - atomic broadcast
    - primitive where message must be delivered at all or none of processes
    - use `(msg, label dests)`; `label` is string of characters representing a
      queue name - to help guarantee ordering
  - causal broadcast
    - broadcast where order is predetermined
    - allow labels for causal broadcasts to still permit concurrency
- allows asynchronous rpcs
