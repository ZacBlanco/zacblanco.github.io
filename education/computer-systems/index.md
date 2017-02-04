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

### Lecture 4 - 1/26/2017

**Switching from User to Kernel Mode**

After a trap it is necessary for the OS to transfer control back from kernel to user mode.

- How do we get back to user mode after trap instructions? (Like system calls)
  - Naive approach: simply set the mode register to 1 then set PC - but then we get an exception before setting PC!
  - Set the PC then the mode bit?
    - Jump to "user-land" , still in kernel mode.

Most machines have a "return from exception" instruction

- A single hardware instruction:
  - Swaps PC and PC'
  - Sets the mode bit to user mode.
- Traps and exceptions use the same mechanism (RTE, Return from Exception)

**Interrupts**

Interrupts are signals sent to the CPU by external devices - normally IO. They tell the CPU to stop its activities and execute appropriate part of the operating system

The three main types are:

- Hardware Interrupts
- Software Interrupts
- Traps

Every OS or machine has a set of code (PC value) set where the code to handle the logic for exceptions/interrupts/traps

**I/O**

I/O or input output devices are hardware devices that the user can typically use to send signals into the system (to perform an action) or out of the system to perform actions such as notifying a user, or writing to a file.

Keyboards, mice, and monitors are all examples of I/O devices.

_A Simple IO Device_

- Network card has 2 registers:
  - A store into the "transmit register" - to send bytes
  - A load from the "receive" register - to receive last byte

How can the CPU access those registers?

- It is mapped into memory space
  - I.e. an access on memory cell 98 doesn't actually access memory but is accessing a register instead.
  - This is called a **memory mapped register**

*Why do we use memory-mapped registers?*

- Stealing memory space for device registers has 2 functions
  1. Allows protected access to only allow the OS to access
      - User programs must trap into the OS to access IO devices
  2. The OS can control devices and move data to/from devices using regular load and store instructions.

Questions to ask:

**Q**: When does the processor check whether an interrupt has occurred  
**A**: Processor checks for hardware interrupts every cycle (on the fetch stage)

- Why do we need to map registers on the NICs (Network Interface Cards) to the main memory.

**Q** How does the OS know if a new byte has arrived? Or how does the OS know when the last byte has been transmitted?

**A**: Status registers. A status register holds the state of the last IO operation. Our network card has 1 status register. 

To transmit, the OS writes a byte into the TX register and sets bit 9 of the status register to 1. When the card successfully transmitted a byte it will set the bit 0 of the status register back to 0.

**interrupt Driven IO** 

- Polling (using a while loop) can waste many CPU cycles.
  - On transmit, CPU slows to the speed of the device.
  - Can't block on receive, so tie polling to clock, but wasted work if there's so RX data.

*Solution*: Use interrupts!

- When network has data to receive, signal an interrupt.
- When the data is done transmitting, use another interrupt.

**Why Poll at All**?

- Interrupts have high overhead:
  - Stop processor
  - Figure out what caused interrupts
  - Save user state
  - Process request.

The main thing we should worry about is the frequency at which IO is occurring vs the amount of overhead per interrupt.

- More input = polling may be better
- Less input = interrupts may be better

**Direct Memory Access (DMA)**

The problem with programmed IO: CPU must load and store all the data into device registers

The data is usually in memory anyways. To address this we need more hardware which allows the device to read and write memory like the CPU.

_Programmed IO vs DMA (PIO vs DMA)_

Overhead for PIO is less than DMA  
- PIO is a check against the status register, then send or receive
- DMA must set up the base, count, etc..


### Lecture 5 - 1/31/17

**Clarifying Interrupts**

- **Interrupt** - An event triggered by a piece of hardware or software that will cause the CPU to stop after the next instruction in order to run a specified set of code to handle said interrupt.

We can break interrupts into three categories:

- Hardware Interrupts
  - I/O
  - Network
  - Failures
- Traps (Software/syscalls)
  - read/write
- Exceptions (Software interrupts)
  - Division by 0
  - Wrong memory access

**Practicality: How to Boot**:

- How does a machine actually start the OS?

Boot protocol:

- CPU executes from a fixed address
- Firmware loads the boot loader
- Boot loader loads the OS

The boot protocol:

The CPU is hard-wired to begin executing from a known address in memory. 
This execution in the CPU then loads the BIOS, sometimes referred to as the bootloader. The bootloader is typically stored in EEPROM, a form of electrically erasable ROM. From here it is able to then decide which location in memory (HDD/SSD) to boot from.

**OS Human Computer Interaction**

The common ways to interact with an OS is either at the command-line or with a visual GUI. Typically OSs are written in C or C++ because of the wide support for C and its history with writing operating systems.

The linux operating system also provides a wide set of libraries which allow programmers and developers to use some of the same system calls that the kernel uses.

**Microkernel System Structure**

- Moves as much as possible from the kernel space into the user space
- _mach_ is an example of a microkernel
- Mac OS X kernel (Darwin) is partially based on *mach*
- Benefits
  - Easier to extend a microkernel
  - Easier to port OS to new architectures
  - More reliable (less code in kernel)

**OS Modules**

Most modern operating systems implement loadable kernel modules
  
  - This uses an object oriented approach
  - Each core component is separate
  - Talks to other processes.

**Layered Systems**

A layered system is asystem which is designed such that the lowest layer (layer 0) consists of the hardware. Layers are built on top of one another which use the libraries and calls used by the layers below and grows outwards toward the user interface.

**Hybrid Systems**

Most modern OSs are not purely modular by design. This is due to challenges faecs by performance security and usability needs. Many designers collaborate together on these systems which results in these hybrid OS architectures.

**Runtime Storage Organization**

- Both the text (program) and data will reside in memory

Each variable must be assigned to a storage class. The hierarchy is described below.

1. Code
2. Global (static) variables
3. Stack(Method variables and parameters allocated dynamically)
4. Dynamically created objects (using the `new` keyword in OOP)

### Lecture 6 - 2/02/2017

**Operating Systems Structures**

- OS provides an environment for execution of programs and services to programs and users
- One set of services that provides functions to the user
  - UI/GUI either a command line or graphical user interface
  - Program execution - The system must be able to load a program into memory
  - Resource allocation

System calls are calls supplied by the OS to interact with the computer hardware. This includes (but is not limited to) file I/O or networking tasks.

Three general methods used to pass parameters to OS system calls

- Simplest: Pass in registers
  - hardware limitation on parameters
- Pass parameters via memory on the stack
- Put into a specified block or chunk of memory reserved for syscall parameters.
  - Limitation on # of parameters

Difference between Program and Process

- Program
  - A set of instructions which may reside in memory or on disk
- Process
  - A program that has been initialized by the OS with globals, stack, and heap memory, a PC register value, and executing instructions.


**Runetime Storage Organization**

- Global variables allocated in globals area at compile time
- Method variables allocated dynamically on the stack.
- Dynamically created object - created dynamically and allocated from the heap
  - objects live beyond method
- Pointer to next instruction in the PC

**Process Control Block**

Each process has per-process state maintained by the OS

- Identification: process, parent process, user, group, etc.
- Execution contexts, registers, threads
- Address space: virtual memory
- I/O state: file handles, communication endpoints
- For every process this information is maintained on a process control block.
  - This is data in the OS memory space

**Process Creation**

Create processes from system calls

- In unix we use `fork()`

The process which creates the new process is called the parent and the new process is the child.
- The child process is created as a copy of the parent process except for the identification and scheduling state.

Parent and child processes run in two different memory address spaces
- By default there is no memory sharing
- Process creation is expensive because we need to copy all variables, globals, etc over to a new process space.

In unix `exec()` is provided for the new process to run a different program other than a parent (this runs a different set of instructions which may be stored on disk)

**Process Death**

You can force a process to wait (stop executing instructions using `wait()` system call)

In Unix you can wait for any process by passing its PID to `wait()`.

- You can kill another process using the `kill()` system call
  - This is done by a signal named SIGKILL in Unix

**Signals**

Signals are a type of software interrupt which is provided as OS system calls to programs in order to interrupt programs from running their instruction and run a different set of instructions to handle the signals.

These signals can be used to kill a program, notify the program of message arrival, or of a certain type of event occurring.

Programs can specify handler functions for certain types of signals. When the OS receives a signal intended for a certain process it will check if the process has a hanler function for the signal. If so it will interrupt the program and run the interrupt handler method instructions with the signal parameters.

In this process the CPU has to switch the stack variables because switching to running the handler function requires a context switch into kernel mode to load the instructions, then back to kernel mode to notify the signal has been handled, then back to the user mode instructions.

**Processes Summary**

- Instantiation of a program

OS process management:

- Supports creation of processes and facilitates interprocess communication
- Allocates resources to processes according to specific policies
- Interleaves the execution of multiple processes to maximize system utilization


































