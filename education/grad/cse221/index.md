---
layout: page
title: CSE 221 - Operating Systems
permalink: /education/grad/cse221/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, rust
description: Notes for CSE 221 (Operating Systems) at UC San Diego
mathjax: true
---

## Lecture 1 - Intro

Checkpoints - things that we need to be familiar for this class

- What is a privileged instruction. Examples?
- Difference between system call and a function call?
- Give an example of an atomic instruction
- Difference between a semaphore and a condition variable
- Difference between hardware and software-managed TLBs
- What can happen at a memory instruction. Can the CPU directly access the cache to get the data?
- What is an inode?

Use the textbook _Operating System Concept, by Silberscatz, Galvin, and
Gagne_ if you need to brush up on OS concepts.

Objectives for the course

- Understanding system design trade-offs. Previous and modern-day systems.
- Gain experience reading research papers
    - Develop intuition for what question and issues are important, which are not.
- Experience discussing research material
    - Be able to express opinions and arguing points for/against design decisions.

## Lecture 2 - "'THE'-Multiprogramming System" and "The Nucleus of a Multiprogramming System"

- [Notes to THE](10-6/the-multiprogramming-system/)
- [Notes to Nucleus](10-6/nucleus-multiprogramming-system/)

## Lecture 3 - TENEX and HYDRA

- [Notes to Tenex](10-8/tenex/)
- [Notes to Hydra](10-8/hydra/)

## Lecture 4 - Protection and Protection in Multics

- [Protection](10-13/protection/)
- [Protection and Information Sharing in Multics](10-13/multics/)

## Lecture 5 - UNIX and Plan 9

- [The UNIX Time-Sharing System](10-15/unix/)
- [Plan 9 From Bell Labs](10-15/plan9/)

## Lecture 6 - Medusa and Pilot

- [Medusa: An Experiment in Distributed Operating Systems Structure](10-20/medusa/)
- [Pilot: An Operating System for a Personal Computer](10-20/pilot/)

## Lecture 7 - Monitors and Mesa

- [Monitors: An OS Structuring Concept](10-22/monitors/)
- [Experience with Processes and Monitors in Mesa](10-22/mesa/)