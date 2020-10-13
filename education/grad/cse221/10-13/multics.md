---
layout: page
title: CSE 221 - Operating Systems - Notes on "Protection and the Control of Information Sharing in Multics"
permalink: /education/grad/cse221/10-13/multics/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, rust, memory, security, protection, mechanism, multics
description: Notes for "Protection and the Control of Information Sharing in Multics"
mathjax: true
---

> Q: Compare and contrast protected subsystems in Multics with procedures in
> Hydra.

Main idea of paper is to share ideas around design of information protection in
multics

- Security - base sec mechanisms off of default access being _disallowed_.
- check accesses to all objects for current authority.
- design is not secret - don't assume attacker ignorance
- principle of least privilege, dont give more access than necessary.
- interface is natural, easy, and simple


- wanted to provide decentralization of permission adding
    - prevent admin from being a bottleneck
    - users might need a protection scheme not in original design
        - system provide a set of functions so the user may construct their own
          protection environment.

Multics itself is design as "an organized information storage system"

- ACL for each segment of data
- If the principal identifier is not in the ACL, reject access.

Three user partitions
- username-group i.e. "zac:zac"
- project group i.e. "zac:wuklab" - users who cooperate on an activity
- compartment - user may operate in a compartment, used at authentication time.
    - useful if "borrowing" a program

objects with ACL can be
- data segments on disk
- message queues
- directories
- removable media

Access modes

| mode | use case |
|-|-|
|none| access denied|
|r| read-only data|
|re| pure procedure |
|rw| writable data |
|rew| impure procedure |

- more specific entries in ACLs are considered first to deny access.
- default ACL to parent directory. User should specify if they want different
  permissions
- providing error messages is a point of providing privacy to the user/system.
- idea to add a "trap" extension to call an interrupt handler to run a custom
  procedure before granting access
    - useful for, say, permitting access only between 9am and 5pm.
    - not feasible due to security concerns on where the trap could run and cost
      of executing custom procedure on every access.

Authentication of Users
- register users with a password
- unauthenticated batch jobs held in a queue until authentication
- mechanism of impersonation
- password generating program
- password not shown on terminal
- auto log out
- addition of thread monitoring log
- 10 pw retries before disconnect
- previous login location printed
- allow full project admin control over processes in the group.

