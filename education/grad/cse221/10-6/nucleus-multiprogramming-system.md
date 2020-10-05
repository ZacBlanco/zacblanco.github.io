---
layout: page
title: CSE 221 - Operating Systems - Notes on "The Nucleus of a Multiprogramming System"
permalink: /education/grad/cse221/10-6/nucleus-multiprogramming-system/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, rust, dijkstra, multiprogramming, nucleus
description: Notes for 'THE'-multiprogramming system
mathjax: true 
---

Question to answer:

> How does synchronization in the RC 4000 system compare with synchronization in
> the THE system?

_Answer_:

Synchronization is achieved via send/wait message/answer calls. Processes are
cooperative, meaning they control their CPU scheduling. Dijkstra uses semaphores
which swaps processes on/off the CPU in a "fair" way so that it can be shared
by more users.


### Abstract

- philosophy of a multiprogramming system with a hierarchy of OSes.
- program execution and IO are uniformly as parallel cooperating processes

- programs on old systems were built for those systems - almost impossible task
  to replace an OS. OS are built "special purpose"
  - batch processing
  - priority scheduling
  - RT scheduling
  - conversational access

Thus, the aim of building the "nucleus" is to make something extensible which
is capable of adding functionality.

The designers wanted to

- Assign precise meaning to the "process" concept
- select synchronization primitives to transfer information among parallel
  processes
- define rules for dynamic creation control, and removal of processes.

### Processes

- internal/external processes
 - (external) program is a set of instruction
 - (internal) is a process governing the execution of the program within a
   storage area

- _peripheral_ is a hardware device with an ID connected to a data channel.
- _document_ is a collection of data stored on a physical medium - punched
  cards, magnetic tape, file, etc.
- _external process_ is the input/output of a given document ID'd by a unique
  process name

Coordination between internal and external processes is handled by the "nucleus"
which is an interrupt response program with complete control of IO, storage, and interrupts.

### Process Communication

- References THE from dijkstra's paper, but concludes "semaphore concept alone does not fulfill requirements of safety and efficiency"
- Processes communicate through message buffers.
- Synchronization via
    - _send message_ - copies message into first available buffer and delivers
      it in the queue
    - _wait message_ - delays the requesting process until a message arrives in
      its queue
    - _send answer_ - copies answer into a buffer of a message which was received, pushing it back to the original sender.
    - _wait answer_ - delays te requesting process until an answer arrives in a given buffer.

Communication is vital, and processes should go undisturbed in the event of errors of
malicious or undebugged programs.

One issue, is that because the pool of buffers is finite, a process may consume
all available buffers - preventing other processes from sending messages.
- A limit on the amount of buffers a process may consume is set.

### External Processes

Communication primitives designed for exchange of messages between internal
processes, but later decided to use for communication between internal and
external processes too.

- For each external process the system contains logic to interpret messages from
  the internal process, initiate IO with the specified storage area.
  - When IO is terminated by an interrupt, nucleus generates an answer to the internal process with message about block size and possible errors.
- External processes created on request from internal processes.
    - Creation is assignment of name to a peripheral device

Synchronization is obtained by sending messages to a clock

### Internal Processes

- Created by other internal processes
- assigned name to a contiguous storage area selected by the parent.
- parent process can start or stop a child. It may also remove the process
- programs themselves have freedom to choose scheduling strategy, so the
processes must cooperate.

### Process Hierarchy

- need control strategies for operator communication, program scheduling and
resource allocation. Should be implemented as other processes.

Arrange processes in a hierarchy where parents have complete control over
children in order to maintain operations. All child processes must fit within
the parent.
- Can be used as a strategy to limit system resources.

### Implementation

- communication is relatively expensive (2ms) to have a "conversation" (send-wait-wait-answer)
- Process accounting (start/remove take ~30ms), create/stop 3-4ms
    - Long times are due to storage protection system of the RC4000?