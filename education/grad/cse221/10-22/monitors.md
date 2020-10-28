---
layout: page
title: CSE 221 - Operating Systems - Notes on "Monitors, an OS Structuring Concept"
permalink: /education/grad/cse221/10-22/monitors/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, rust, memory, OS, sync, monitor, java, semaphore, lock, mutex
description: Notes for "Monitors, an OS Structuring Concept"
mathjax: true
---

> Q: What are "monitor invariant" I and "condition" B, and why are they
> important in the discussion of monitors?



### Introduction

- aim of OS is to share computer installation among many programs with
  unpredictable resource demands.
- scheduler consists of an amount of local administrative data w/ procedures and functions called by programs.
    - Collection known as a _monitor_

![](../2020-10-21-09-47-09.png)

- Call monitor by `monitorname.procname(parameters)`
- procedures on monitors are identical to data representation classes
  exception for the name _monitor_.
- The difference is that while any program may at any time call a monitor's
  procedure - only one program at a time may enter the monitor's procedure.
- procedures of monitor should not access non-local variables.
- useful in the case of requesting resources.
- if within a monitor and need to wait for program to release resource we
  require a `wait` and `signal` operation to stop and resume execution
  respectively.
- `wait` operations relinquish exclusion on entering the monitor procedure.
    - signalling after a wait causes the exclusion to happen again.
- use a `condition` variable type for each possible reason that a program may
  need to wait.
- condition variable holds a queue of processes currently waiting on the
  condition.

![](../2020-10-21-10-05-05.png)

### Interpretation

- Semaphores implemented by a monitor above, now implement a monitor with a
  semaphore
- use `mutex` semaphore to exclude monitor programs
- use `urgent` to maintain count of waiters
- if urgent is > 0, don't unlock the mutex, increment urgent only
    - otherwise, unlock mutex
    - prevents other threads from entering monitor urgent while mutex is held
- third variable `condsem` initialized to 0 for process to suspend itself when
calling `wait`.
- this section is very confusing - not well explained.

wait implemented as

```
condcount++;
urgentcount > 0 ? V (urgent) : V(mutex)
P(condsem) // this always waits
condcount--;
```

signal implemented as

```
urgentcount++;
condcount > 0 ? V(condsem); P(urgent) : ();
urgentcount--;
```

- typically wait due to a condition
- when signalling it must be true that the waiter is waking because the
  condition is true.
- possible to deadlock, upt to programmer to fix.
- as an extension, allow the priority of a waiting process to be defined.
    - no effect on logic of program
    - ordering of waiting queue is fast typically
    - max amount of storage requires is 1 word/process.
    - `condition.queue` -> true if anyone is waiting on `condition`
- another extension: allow a method to return whether or not there are waiters.
- designed to be a primitive OS construct
- still a question on whether last instr. should be a `signal` or not.


## Lecture Notes

- Propose monitor as a way to structure the OS
- what's included in monitor?
  - set of methods and data
  - lock to enter monitor
  - queue to enter monitor
  - condition variables to synchronize processes inside of the monitor
