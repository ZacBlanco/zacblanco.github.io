---
layout: page
title: CSE 291 - Dependable Systems
permalink: /education/grad/cse291-dependability/
keywords: CSE, 291, dependable, dependability, UCSD, UC, San Diego, software, operating, system, database, YY, Zhou, dependable, facebook, google,
description: Graduate Course in System Dependability
mathjax: true
---




## Lecture 3


- most software errors are not due to hardware
- most common source of errors is misconfiguration and software bugs
- general ordering:
  - misconfig
  - bugs, operational
  - system problem

- software faults:
  - static defect in software
  - failure: external, incorrect behavior w.r.t requirements or descriptions of behavior
  - error: incorrect interfnal state manifested by a fault

- software fault tolerance
  - large complex system, fault avoidance is difficult:
  - rule of thumb: fault density is 10-50 per 1k LoC. 1-5 after intensive testing+tooling
  - redundancy in SW needed to detect, isolate, and recover from SW failures
  - HW fault is easier to assess
  - SW is difficult to prove correct

- HW faults:
  - time dependent
  - duplicate hardware detections
  - random failure is main cause

- SW faults:
  - faults are time-invariant
  - duplicate SW not effective
  - complexity is the main cause

- fault removal
  - system not exercised
    - behavior model checking
    - static analysis
    - theorem proving
  - system exercised
    - dynamic verification
    - symbolic inputs -- symbolic execution
    - actual inputs -- testing


- verification (e.g. symbolic execution) doesn't scale well
- conventional testing can show presence of bugs, but not their absence

- SW testing
  - exhaustive testing is impossible
  - approach is to define equivalence classes of inputs -- so one case from each class suffices
  - techniques:
    - path testing, branch testing, interface testing, special values testing, functional testing,
      anomaly testing
    - studies show path and interface testing are difficult to design but afford good coverage for
      most applications

- n-version programming
  - build separate programs of the same software, make sure that they all agree on outputs

## Lecture 4

- cloud benefits
  - 0 upgront capital
  - eliminates operational responsibilities
  - eliminates knowledge of "where" logic is running
  - substantial elasticity and scalability
  - economies of scale
- cloud models
  - SaaS
  - PaaS
  - IaaS

- deployment models
  - private cloud
  - community cloud
  - public cloud
  - hybrid cloud

- management models
  - self-managed
  - 3rd-party manage

  
