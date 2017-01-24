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












