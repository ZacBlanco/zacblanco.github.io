---
layout: page
title: CSE 221 - Operating Systems - Notes on "Lottery Scheduling; Flexible Proportional-Share Resource Management"
permalink: /education/grad/cse221/11-19/lotto/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, OS, fs, unix, scheduler, parallelism, kernel, lottery
description: Notes for "Lottery Scheduling; Flexible Proportional-Share Resource Management"
mathjax: true
---


> Q: At a high level, as a mechanism how does lottery scheduling provide modular
> resource management? Why can't the Unix scheduler provide a similar kind of
> management?

## Introduction

- published 1995
- lottery-share scheduling
    - control over relative execution rates
    - proportional-share resource management
    - can be generalized to mutexes, IO, etc
- resource rights represented by _lottery tickets_
- allocation is determined by holding a lottery
- resource rights
    - tickets are relative as the fraction of a resource they represent varies
      dynamically with number of ticket issued
- lottery scheduling is _probabilistically fair_
- small scheduling quantums (< 10ms) can achieve good throughput on sub-second
  time intervals

## Modular Resource Management

- Ticket transfers
    - movement of tickets from one client to another
    - transfer during dependency, e.g. blocking on an RPC
- ticket inflation
    - don't let clients create more lottery tickets --> distorts probabilities.
    - however, can be useful to adjust allocations without communicating.
- ticket currencies
    - _currency_ denominates tickets within a trust boundary
    - a _currency_ is funded by tickets denominated in more primitive currencies
    - contain inflation via _exchange rates_
- compensation ticket
    - a client consumes a fraction _f_ of its allocated resource quantum can be
      granted a compensation ticket that inflates value by 1/f until the next
      quantum that it runs

## Implementation

- Random numbers
    - need fast RNG - impl in 10 RISC instr.
- lotteries
    - simple: traverse client list until finding winner --> O(n)
    - more complex: tree of partial sums, traverse to find winner on O(log(n))
- Ticket Currencies
    - tree-based structure to compute currencies
    - active or non-runnable threads have their currencies deactivated
- ticket transfers
    - mach_msg syscall modified to temporarily transfer ticket from client to
      server for RPCs

## Experiments

- Experiments are in-line with probability calculations based on the number of
  tickets issued.
- used monte-carlo simulations
- implemented a server application which received RPCs and ticket allocations in
  order to process requests
    - can use to provide different levels of service to a particular client.

- mutex funding allows fast task scheduling by giving mutex holder tickets of
  all waiting threads on a mutex
- multiple resource funding
    - one idea: separate manager thread for each application


## Lecture Notes

- main point
    - resource management of CPU similar - applies to all resources
    - use a probabilistic mechanism for proportional-share scheduling
    - motivations
        - want to express relative priorities
        - multi-user servers
        - multi-programmed workstations
        - soft realtime applications (multimedia)
        - overhead of existing fair-share scheduler
        - desire to have general CPU resource mgmt system
- how does it work?
    - each process has a certain number of lotto tickets, each representing one
      CPU quanta
    - OS holds lotteries to schedule on a quanta
    - holder of winning ticket gets CPU
    - More tickets -> more likely to get CPU quickly and more likely to get CPU
      over time if a user has _t_ tickets out of a total _T_ tickets.
        - probability of winning is $$p = t/T$$
- why tickets?
    - abstract, independent of machine speed or details
    - relative: fraction of resources allocated varies in proportion to contention
    - starvation free
    - also: algorithm oblivious of past behavior (less metadata updates)
- other benefits
    - can transfer tickets
        - handle priority inversion problems of client/server model.
    - inflation/deflation of tickets
    - currencies --> hierarchical allocations between trusted boundaries.
    - compensation tickets.
- impl
    - RNG in 10 instructions: RNG doesn't need to be that good
    - basic: maintain linked list of clients ordered by # of tickets held
- issues
    - can't express response time differently from CPU share
        - e.g. event-activated work: needs to execute quickly but not a lot of
          CPU
    - 