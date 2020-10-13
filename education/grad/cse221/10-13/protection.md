---
layout: page
title: CSE 221 - Operating Systems - Notes on "Protection"
permalink: /education/grad/cse221/10-13/protection/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, rust, memory, security, protection, mechanism, policy
description: Notes for Protection
mathjax: true
---

> Q: What are the concepts in HYDRA that correspond to Lampson's definitions of "Domain", "Object", and "Access Matrix"? What about Multics?

- Security mechanisms are far and wide
    - user/kernel mode
    - memory bounds checking registers
    - filesystem ACLs

- Security should be abstract. Many previous mechanisms focused on user
  separation. More now on program separation. Safety mechanisms enable
  feasibility of large systems as well as commercial applications.

Message passing system analogy
- processes separated by hardware (inefficient)
- message from A to B is ok
    - what if B is malicious or unreliable? Wait for a message from C which _is_ reliable.
        - Q: but how can we verify C is reliable if B isn't?
- Each process gets the identity of the caller, so it can accept or reject.
- Verifying identity is difficult

- Object system
    - Set of domains
    - access matrix

Contains objects
- definition of object up to reqs of a system (i.e. processes, domains, files, segments)
- Access of domains to objects is determined by an access matrix
- each element of the matrix specifies the a set of strings, "access attributes", (e.g. `rwx`) which
label the access abilities of a domain for a given object

Can implement capability lists with a matrix of $$ A[d, A[d, x]] $$

- Generalized idea of users v files access with ACL or capability list
- users or files can be "domains", a set of access abilities exist under each combination of domains.