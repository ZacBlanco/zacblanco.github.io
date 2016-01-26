---
layout: page
title: Computer Architecture 
keywords: engineering, rutgers, spring 2016, computer, undergraduate, MIPS, qtSpim, computer, organization, nagmeh, karimi, notes, succeed
permalink: /education/comp-arch-332/notes/
mathjax: true
author: Zac Blanco
---

These are notes for the version of the class which is being taught by professor Naghmeh Karimi in the Spring of 2016.

- Textbook: D.A. Patterson and J.L. Hennessy, Computer Organization and Design: The Hardware/Software Interface - 5th edition

- Grading:
  - Homework and Quizzes: 30%
  - Midterm: 30%
  - Final: 40%
  
## Course Goals

- Ability to define the principles of Computer Architecture
- Ability to write assembly programs
- In-depth understanding of integer and floating point architectural blocks
- In-depth understanding of the datapath inside a processor
- In-depth understanding of pipelining for 32-bit architectures and pipeline hazards
- Ability to understand and analyze computer memory hierarchy
- Ability to understand computer busses and input/output peripherals

## Lecture Topics

- Intro, History of Computers
- Assembly Instructions
- Processors (datapath and controller)
- Memory Hierarchy
- I/O devices
- Multi-core


## Lecture 1 - Introduction

#### 1958: First integrated circuit
  
  - Flip-flop using two transistors @ Texas Instruments
  
In 50 years we went from 2 transistors to 55 million! This was driven by the miniaturization of transistors.

Computers have been increasing in speed and decreasing in size (really the transistors have!) since their inception in the 1950's and 60's

It has been followed by Gordon Moore's Law (more commonly known as Moore's Law) that the amount of transistors that could fit on a silicon die would double every 2 years. Surprisingly his law holds true almost all the way from the 1960s when he first predicted this, all the way until now (2014/2015)!

However one downside to this is that the power consumption of these chips has been increasing as well.

## Lecture 2 - Introduction 2

We want to look at how these computers are structured and organized though. What exactly does it do?

- Instruction set, registers
  - MIPS, x86, Alpha, PIC, ARM
- Microarchitecture
  - Single cycle, multi-cycle, pipelined, superscalar?
- Logic: how are functional blocks constructed?
  - Ripple carry, carry look-ahead, carry select adders
- Circuit: how are transistors used?
  - Complementary CMOS, pass transistors, domino
- Physical
  - Chip layout

There are different classes of computers which we should consider.

- Desktop Computers
  - Designed to deliver good performance for a single user at a low cost. Typically use mouse and keyboard
  
- Servers
  - Used to run larger programs for multiple simultaneous users typically accessed only via a network and that places a greater emphasis on dependability and often security
  
- Supercomputers
  - A high performance, high cost class of servers with hundreds to thousands of processors, terabytes of memory and petabytes of storage that are used for high-end scientific and engineering applications

- Embedded Computers (Procesors)
  - A computer inside another device used for running one predetermined application

What happens below a program in a computer?

Usually it's written in a high-level language which was translated (compiled) from the programming language to machine code. From there the operating system is able to take the machine code and turn the machine code to service code which handles the input and output, memory management and storage, and scheduling tasks and sharing resources. 

All of these instructions are then sent to the Hardware (processor, memory, I/O controllers).

Most of this information in a computer is all translated into different languages. Usually you start at a High-Level language (c/java), it is then turned into assembly language, a textual representation of instructions for I/O and arithmetic, etc.. Assembly is then translated into binary bits (1's and 0's).

An **assembler** is analagous to a compiler in that it translates the assembly language into binary machine languages.

Computers are really just large abstractions of many different components of a computer. I/O components

- Controllers need to have:
  - Ability to input instructions from memory
  - logic and means to control instruction sequencing
  - logic and means to issue signals that control the way information flows between datapath components
  - Logic and means to control which operations are performed
- Datapath needs to have:
  - Components - functional units (e.g. adder) and storage locations (e.g. register file) - are needed to execute instructions
  - Components interconnected so that the instructions can be accomplished
  - Ability to load data from and store data into memory
  
  1. Processors _fetch_ instructions from memory
  2. The controller decodes the instruction to determine _what_ to execute
  3. Datapath will then _execute_ the intruction as directed by the controller
  4. Upon completion the data output will be _stored in memory_
  5. The output device will then _output_ the data

But what exactly is the interface between the software and the hardware?

It is the **instruction set**

### Instruction Set Architecture (ISA)

- It is the actual programmer-visible instruction set
- Boundary between software and hardware
- Decisions regarding
  - Class of ISA
    - Register memory ISA (memory can be accessed by many different instructions)
    - Load-store ISA (only load/store instructions can access memory)
  - Memory addressing (byte addressable)
  - Addressing modes (register, immediate, displacement, ...)
  - Instruction operands (size/type)
  - Operations (data transfer, arithmetic/logical, control, floating point)
  - Control flow instructions (connditiona/uncondfitional branches, call/return)
  - Instruction encoding (fixed/variable length)



## Lecture 3 - January 26th 2016

Most of the time when we think performance we think "execution time" or "response time". For a computing system, or processor. There are many factors of the processor design which can affect the time it takes to a execute a specific program.

Generally the response time is proportional to the _clock speed_ and _instructions per clock cycle_.

Maximizing performance means minimizing execution time.

We can give a general comparison of the speed of two processors running the same program by using the following equations.

> $$ \frac{Performance_x}{Performance_y} = \frac{Time_x}{Time_y} $$

Example: if A runs a program in 10 seconds and B runs the same program in 15 seconds, then A is 1.5 times faster than B.

In measuring the exact time it takes to run a program, we can take a very basic understanding that we can run _x number of instructions per cycle_, _how many instructions we need for a whole program to run_, and finally _the number of seconds for a single clock cycle_.

This will give us that program run time is:

> $$ \text{CPU Time} = \frac{\text{Clock Cycles}}{\text{per Instruction}}\times\text{# of Instructions}\times\frac{Time}{\text{per clock cycle}} $$

One important thing to note with this abstraction is that we assume every instruction takes the same amount of time. This is necessarily always true. Generally a load or retrieval from memory will take a longer amount of time














































