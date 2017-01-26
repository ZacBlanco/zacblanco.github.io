---
layout: page
title: Introduction to Computer Systems
permalink: /education/computer-systems/
keywords: 14:332:452, rutgers, computer, systems, engineering, electrical, computer, operating systems
description: A page full of notes and useful links for succeeding in Intro to Computer Systems at Rutgers University.
mathjax: true
---

These notes are for a version of the class taught by Prof. Maria Striki in the Spring of 2017.

The textbook for this course is: Silberschatz, Galvin, and Gagne. Operating System Concepts. John Wiley & Sons

Reading assignments are based off of the 9th edition. Any of the 6th, 7th, or 8th edition should be okay.

Grading:

- Midterms: 24%
- Final: 24%
- Projects: 38%
- Homeworks & Quizzes: 14%

## Lecture 1 - 1/17/17

Goals of this course:

- Understanding of OS and the OS/architecture interface/interaction
- Learn a small amount about distributed OSs


This course covers:

- Processes and Threads
- Synchronization
- Processor Scheduling
- Virtual Memory
- File Systems
- I/O Systems
- Security
- Distributed Systems

**Programming Assignments (tentative)**

- Shell & System Calls (Linux Kernel)
- Partial Threads package & multi-threaded server
- File System (Linux Kernel)

What is an operating system?

- A software layer between the hardware and the application programs/users which provides a *virtual machine* interface - easy and safe.
- A *resource manager* that allows programs/users to share the hardware resources: fair and efficient.
- A set of utilities to simplify application development and execution.

## Lecture 2 - 1/19/2017

We're going to look at a computer from an OS designer perspective

- The operating system is a layer of software that creates a virtual machine
- The OS also manages the resources of this machine but this mostly involves sharing policies so it will be discussed later

Hopefully we will be able to familiarize ourselves with

- The underlying machine
- Extra hardware mechanisms needed for virtualization.

Topics:

- Von-Neumann architecture
  - CPU + Memory
Hardware support for abstracting the basic machine
  - Modes, Exceptions, Traps, and Interrupts
- Input and Output
  - Network, storage, and graphics


The simple conceptual model of a computer we tend to look at the memory one giant byte array. The CPU is the place where calculations are performed.

- A computer is a piece of hardware which runs the fetch-decode-execute loop.

All Von Neumann computers follow the same loop:

- Fetch the next instruction from memory
- decode the instruction to figure out what to do.
- Execute the instruction and store the result.

Instructions are simple. Ex:

- Increment the value of a memory cell by 1.
- Add the contents of memory cell X and Y together and store it in Z
- Multiply the contents of memory cells A and B and store the contents in B.

How can we represent instructions as numbers?

In a 32-bit architecture you might get 8 bits to store the operator, 8 bits for the operands, and 8 bits for the destination location. Where each 8 bit location can represent a range of 256 values to represent different locations, registers, operations, etc..

First assume a memory cell is 8 bits (1 byte) wide. With this simplified 32-bit architecture we get a maximum total memory size of 256 bytes because we are only allowed $$2^8$$ different memory locations. Thus 256 bytes of memory can be accessed. We used 4 bytes per instruction.


**The Program Counter**

- Where is the "next instruction" held?
  - In a special memory cell in the CPU called the program counter (PC). This is a special purpose memory.

In a naive fetch cycle, we increment the PC by the instruction length (4) after each execute when we assume all instructions are the same length.

**Memory Indirection**

We can efficiently access array elements by using LOAD instructions which allow us to move data between memory cells.

**Conditionals + Looping**

These are instructions which modify the program counter

- Conditionals
  - If the content of this cell is: positive, not zero, etc.. execute the instruction or not
- Branch Instructions
  - If the content of this cell is: zero, nonzero, etc. set the PC to a certain location
- Jumps are unconditional branches

**Registers**

Architecture ule: large memories are slow. Small ones are fast.

- But everyone wants more memory!!
  - Solution: Put small amount of memory in the CPU (cache)
  - Most programs work on only small chunks of memory at a given time so we can still perform quickly due to principles of data locality.
  - By caching the contents of a small number of memory cells in the CPU cache we can execute a number of instruction before having to access memory
- Small memory in the CPU is named separately in the instruction from "main memory"
  - Small memory in CPU - registers
  - Large memory - "main memory"

- Most CPUs have 16-32 "general purpose" registers
  - These registers all look the same. A combination of operators, operands, and destinations are possible.
- Operations and destination can be in
  - registers only (Sparc, PowerPC, MIPS, Alpha)
  - Registers & 1 memory operand (x86 and clones)
  - Any combination of registers and memory (Vax)
- Only memory operations possible with register-only machines are load and store to/from memory.
- Operations 100-1000 times faster when operands are in registers vs memory.
- Saves instruction space too (only address 16-32 registers rather than GB of memory.)

### Lecture 3 - 1/24/2017

**Symmetric Multiprocessing Architecture**

Typically in a multi-CPU architecture there are multiple CPU cores which each have their own registers and own cache, but all share the same main memory

Clustered Systems

- Like multiprocessor systems, but multiple systems working together
  - Usually sharing storage via storage area network (SAN)

**Operating System Structure**

- Multiprogramming (batch system) - needed for efficiency
- Single user cannot keep all CPU and I/O devices busy
- Multiprogramming organizes jobs

**Transition from User to Kernel Mode**

Example

User process executing --> System call --> transfer to kernel mode (trap, mode bit = 0) --> execute system call in kernel mode --> return mode bit = 1 --> return from system call 

- Timer to prevent infinite loop/process hogging resources
  - Time is set to interrupt the computer after some time period
  - Keep a counter that is decremented by the physical clock
  - Operating system sets the counter (privileged instruction)
  - When counter hits 0 generate an interrupt
  - Set up before scheduling process to regain control or terminate program that exceeds allotted time.

**Process Management**

- A process is a program in execution. It is a unit of work within the system. Programs are passive entities. Processes are active entities.
- Processes need resources to accomplish tasks.
  - CPU, memory, I/O, files
  - Initialization data
- Process termination requires reclaim of any reusable resources.
- Single-threaded processes have one program counter which specifies the location of the next instruction to execute.
  - Processes execute instruction sequentially, one at a time until completion.
- Multi-threaded processes have one program counter per thread.
- A typical system has many processes, some user and some OS processes which run concurrently on one or more CPUs
  - Concurrency is performed by multiplexing the CPUs among the processes and threads.

The OS is responsible for:

- Scheduling processes and threads on the CPUs
- Creating and deleting both user and system processes
- Suspending and resuming processes
- Providing mechanisms for process synchronization
- Providing mechanisms for process communication

**Memory Management**

The operating system is responsible for the following activities in connection with memory management:

- Keeping track of which parts of memory are currently being used and who is using them
- Deciding which processes (or parts of processes) and data to move into and out of memory
- Allocating and deallocating memory space as needed

*Simple Protection Scheme for Memory*

All addresses < 100 are reserved for operating system use

Mode register is also provided

- 0 = CPU in OS/Kernel Mode
- 1 = CPU is in user mode

Hardware check

- on every fetch, if the mode bit is 1 and the address is less than 100, the CPU does not execute the instruction.
- When accessing **operands**, if the mode bit is 1 and the *operand address* is less than 100, do not execute the instruction
- The mode register can only be set if the mode is 0.

**Fetch-Decode-Execute Revisited**

**Fetch**

    if (PC < 100 && mode_reg == 1) then
        ERROR - user tried to access OS
    else
        fetch instruction @ PC

**Decode**

    if (destination_reg == mode && mode_reg == 1) then
        ERROR - user tried to set mode register
    ....

**Execute**

    if (operand < 100 && mode_reg = 1) then
        ERROR - user tried to access OS
    else
        execute instruction

*Exceptions*

Exceptions occur when the CPU encounters and instruction which cannot be executed (as seen in the example above)

- We modify the fetch-decode-execute loop to jump to a known location in the OS when an exception occurs.
- Different errors will cause execution to different places in the OS


- On fetch memory violation - 60 is a common location to set the PC
- On Decode, 64 is the common location for invalid access

*Access Violations*

Notice both instruction fetch from memory and data access must be checked.

- The execute phase must check both operands
- The execute phase must check again when performing an indirect load
- This is a primitive memory protection scheme. We'll cover more complex virtual memory mechanisms and policies later.


*Recovering From Exceptions*

The OS can figure out what caused the exception from the entry point.

- But how can it figure out where in the user program the problem was?

Solution: add another register - the PC'. When an exception occurs, save the current PC to PC' before loading the PC with a new value. Then jump back to the old PC after finished recovering.

*Traps*

How does a user program legitimately access OS services?

- Solution: Trap instructions

A trap is a special instruction that forces the PC to a known address and sets the mode into system mode.

Unlike exceptions, traps carry a couple arguments to the OS. They are the foundation of the *system call*

- How does the OS know which service the user program wants to invoke on a trap?
  - The user program passes the OS a number which encodes which OS service it is requesting.

Ex. The machine could include the trap ID in the instruction itself.

Most real CPUs have a convention for passing the trap code in a set of registers.

































