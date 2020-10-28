---
layout: page
title: CSE 221 - Operating Systems - Notes on "Experience with Processes and Monitors in Mesa"
permalink: /education/grad/cse221/10-22/mesa/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, rust, memory, OS, monitor, dijkstra, mesa, xerox, PARC
description: Notes for "Experience with Processes and Monitors in Mesa"
mathjax: true
---

> Q: Compare and contrast synchronization in Java with Hoare monitors and Mesa monitors.


- Hoare monitors, the thread signalling yields to the the thread woken
up by the signal
- Mesa monitors, the signalling thread continues, then once it exits the
woken up thread can continue


- paper about
    - local concurrent programming
    - global resource sharing
    - replacing interrupts
- published work on monitors is not complete
- issues with:
    - program structure
    - creating processes
    - creating monitors
    - wait in a nested monitor call
    - exceptions
    - scheduling
    - IO
- pilot needed to choose shared memory (monitors) or message passing
- voluntary scheduling ensures mutual exclusion
    - can't really do this because we need to respond to interrupts in a
      timely manner.
    - nonpreemption restricts programming generality (need yields)
- decided semaphores incur to _little_ discipline on programs


### Processes

Example:

```
p := fork(ReadLine[terminal])
buffer := join p
```

- process can be treated as a variable
- method for passing parameters for processes is the same
- no declaration is needed for a procedure invoked as a process
- cost of creating and destroying is minimal
- detach process with `detach[p]`
- root procedure must be able to handle exception of another process

### Monitors

- monitors unify
    - synchronization
    - shared data
    - code performing accesses
- random order should be acceptable for calling entry procedures into
  the monitor
    - if not suitable, then other provisions to the program should be
      made outside the monitor.

#### monitor modules

- simplest monitor in Mesa is a _module_ - basic unit of global
      program structure.
- three procedure types with monitors
    - entry (execute within monitor block)
    - internal (private, execute within monitor block)
    - external (public)
- external methods might not need to access shared data.
- if an entry procedure generates an exception, the result calls on the
  exception handler from within the monitor - lock will not be released.
  (different from Java?)
    - handler must not invoke monitor --> deadlock
- on exception, restore invariant, releasing lock, then generate exception

#### Monitors and Deadlock

- 3 patterns of deadlock.
    - simplest deadlock in single monitor with two processes doing a
      `WAIT`, each expecting to be awakened by another.
    - 2nd kind, two monitors, M, N. M calls N, N calls M. Deadlock
    - 3rd kind: M calls N, N waits on a condition only true if another
    process enters N through M - can't happen because M is already locked.
- don't blame the tools for bad use

#### Monitored Objects

- use a single monitor for access to a lot of data
    - unfortunately drastically reduces concurrency
    - give each object/data its own monitor instead?
- monitor specifies the object it locks.
    - still not safe since the monitor's pointer to object can change
    - if changed, the return does not release the lock for the monitor
      when it first initially entered a call

#### Abandoning Computation

- Suppose a number of procedures $$P_1 ... P_n $$, if $$P_n$$ generates
an exception, the computation must unwind to to $$P_1$$ unlocking all monitors.

### Condition Variables

- waiting condition variable must run immediately when another process signals.
- signalling process runs as soon as the waiter leaves the monitor.
    - waiter gets to assume truth of some predicate
- mesa does this differently
    - when a process establishes a condition, it notifies the condition var.
        - notify is simply a hint to a waiting process
            - does not guarantee another process won't enter the monitor before
              it wakes up.
- use a loop now instead to wait on a condition $$c$$
    - no constraints on when process must run after a notify
    - can lead to unfairness.
- however, lazy scheduling on `notify` calls allows a few more things
    - timeouts (stop waiting after some period of time)
    - aborts (throw exception)
    - broadcast (notify/wake all)

#### Naked Notify

- Communication with IO devices is handled by monitors and condition variables
  as well.
- use a notify (without getting the monitor exclusively) - kind of like an
  interrupt
- don't have a monitor for device hardware, as its typically much slower and
can prevent a process from running on a fast CPU (compared to disk)
- avoid exclusion between mesa code and device hardware + microcode.

#### Monitor Priorities

- Ordering implied by priorities can be subverted by monitors
- 3 processes each with different priorities, depending on wakeup order and
  queueing, can prevent a higher priority process from running.

### Implementation

- implementation split aong the mesa compiler, runtime package, and underlying
hardware.

#### Processor

- process states kept in a table known to the processor. Fixed table and size gives the maximum number o processes. At any given time, a process is on a queue
    - ready
    - monitor lock queue
    - condition variable queue
    - fault queue
- Queues stored by process priority. (simple circular linked list)


#### Runtime Support

- Process module in mesa forms creation and deletion in mesa, written as monitor.
    - fork and join as entry procedures to a monitor


#### Compiler

- compiler recognizes syntactic constructs for processes and monitors - emits
appropriate code for monitor entry/exit

### Performance

- monitor entry/exit is about 50 ticks compared to 30 for procedures
- context switch is 60 ticks
- wait is 15 ticks
- notify
    - 4 ticks if no one is waiting
    - 9 ticks if someone is waiting


## Lecture Notes

- Language provides "implicit" lock associated with an object
- e.g. `synchronized` keyword to prevent concurrent access to an object.
- main point: describe experiences using monitors in a real OS and design
changes that they made.

- Changes to Hoare's monitors
  - program structure
  - dynamic process creation
  - dynamic monitor creation
  - nested wait
  - exceptions
  - scheduling
  - I/O

![](2020-10-22-09-47-16.png)

| Properties | Hoare | Mesa | Java |
|-|-|-|-|-|
| monitor lock | all procedures | entry procedures | synchronized |
| Condition vars (Y/N) | Y | Y | kind of (one cond. var per object) |
| Wait (Y/N) | Y | Y | Y |
| Signal (Y/N) | Y | notify/broadcast | notify/broadcast |
| Granularity | module | monitor class + monitor record | monitor/instance sync. blocks |
| Aborts | ? | Y | Y |
| Nested Calls | Y | Y | Y |


- Semantics of wait/notify in Hoare's and Mesa
  - Hoare's
    - Signal immediately transfers control to awakened process
    - return from wait implies an invariant holds, so condition does not have to be checked
      - e.g. if (not invariant) wait (c)
  - Mesa
    - notify places processes on the run queue, but does not switch control
    - cannot make assumptions about state returning from wait
    - must check invariant again
    - e.g. while (not invariant) wait (c)

- Hoare monitor recommended priorities inside of monitors
- mesa priority inversion:
  - because the OS handles scheduling, threads at whim of the OS scheduler on
    the monitor.
  - low priority processes need to leave process ASAP so the high priority
    process can enter.
      - Can quickly boost low priority process in the monitor at the OS-level to
      get it to quickly be scheduled, and then finish
