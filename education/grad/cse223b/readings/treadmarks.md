

## Introduction

- Distributed shared memory for Unix systems
- distributed computing allows for low-cost expansion
- "shared memory easier than message passing paradigm"


## Design

- focus on reducing communication necessary for consistency
- _release consistent_ memory
    - relaxed model which allows delayed changes to a page until a particular
      instruction occurs
    - _lazy RC_: updated is postponed until another acquire comes along
- _multiple-writer protocols_ to address "false sharing"
    - false sharing: two or more processors access different variables within a
      page, with at least one as a write
    - causes unnecessary communication
    - multiple-writer-protocol for two or more procs at the same time to modify
      a page, then combine the diffs
- _lazy diff creation_ (to record updates to pages)
    - diff is an RLE page representing the modifications to the page.
    - diff creation occurs when processor requests modifications to a page or a
      _write notice_ message arrives at the processor


## Implementation

- 3 main data structures:
    - page array
        - points to LL of write notices for the page
        - entry in LL for each shared page with pointers to interval records and
          diff pool
    - diff pool
        - data structure presenting the RLE diff
    - process array
        - maps processes to interval records.

### Interval and Diff Creation

- new interval at each release and acquire
- new interval creation postponed until communicating with another process
    - avoid overhead if lock is reacquired by same processor

### Locks

- locks use a statically assigned manager
- uses round-robin algorithm
- lock acquire requests contain the vector timestamp
    - at release, the releasing processor informs any acquirer of all intervals
      between the vector timestamp in the lock request and the releaser's
      current vector TS
- after receiving response message back, acquirer incorporates new information
  into its data structures


### Barriers

- barriers have a centralized manager
- centralized manager collects the vector timestamps from the client
- once all barrier messages are received, the manager sends messages back to the client.

### Access Misses

- faulting processor does not have a copy of the page, request a copy from a member of the page's copy set. If write notices are present, then faulting processor obtains missing diffs and applies them to the page.
- follow linked list of write notices to obtain all page diffs
    - optimize by: if processor p modified a page during an interval, then p must have diffs of all intervals with a smaller timestamp than the interval. Find the largest interval of each processor with a write notice but no diff.
- request all diffs in parallel, apply to page in order

### Garbage Collection

- need to reclaim space used by write notices, intervals, and diffs
- processor validates copy of every page modified
- update copyset for each page.
- request a copy for a page if it doesn't have it after a GC
- GC triggered when amount of free space for consistency drops below a particular threshold.

## Performance

- minimum RTT is 500us
- 80us spent in software and kernel on send+receive
- time to remote acquire lock is 827us (acquirer was last process), 1149ms (otherwise)
- barrier us about 2ms
- some performance could be gained with a kernel-based instead of user-level implementation
    - execution time using different network (e.g. ethernet) increases significantly

