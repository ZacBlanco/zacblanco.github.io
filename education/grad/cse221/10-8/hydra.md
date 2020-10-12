---
layout: page
title: CSE 221 - Operating Systems - Notes on "HYDRA - The Kernel of  Multiprocessor Operating System"
permalink: /education/grad/cse221/10-8/hydra/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, rust, multiprocessor, hydra
description: Notes about Hydra
mathjax: true
---

Question:

> How is a Hydra procedure different from the procedures we are familiar with in
> a typical language and runtime environment?

Answer:



### HYDRA

- Use C.mmp multiprocessor, 16 cores (PDP-11's), 32 million addressable bytes

Specific considerations for Hydra

1. Multiprocessor environment
    - not many implementation of multiprocessor systems - should be conservative
2. Separation of mechanism and policy
3. Integration of design with implementation methodology
4. Rejection of strict hierarchical layering
5. Protection
6. Reliability

3 critical concepts

- Procedure
- LNS (Local namespace)
- Process

- _capability_ consists of a reference to an object together with a collection
   of "access rights" to that object.

- Make a procedure call (system call?) which creates a new LNS from the callee
based on capability.

- _Process_ is the smallest entity schedulable for execution
- argue hierarchical structures are bad for security
- each object has a capability which is only modifiable by the kernel and data,
  representing the data behind the object


kernel provides the _walk_ primitive
- input: capability and nonnegative integer
- output: the capability that occupies the cap in the part of the object named
  by the parameter capability


  Users have access to procedures, which also have access to some amount of object data

### Notes from Lecture

- Hydra is based on security and capabilities
- separating mechanism (the ability to do an action) from policy (condition
  enforcing a mechanism).
-

Most systems today use ACLs, hydra proposes capabilities

- ACL
    - example: Files today have ACLs which are owner, group, other, with
      policies RWX. When showing up to perform a mechanism, need to show identity
      in order to perform an action
    - Data stored on the _object_ with the list of "people" who can
      access and what they can do.
- Capabilities
    - Issued a key which "unlocks" ability to perform action on a number of
    different objects
    - Data associated with available actions is a list of objects with
      the specific actions allowed for that user.

Local Namespace

- Represent an execution domain
- contains list of pointers (capabilities) that can be accessed by that namespace

Procedures
- code, operations, and objects
- templates: define type and capability