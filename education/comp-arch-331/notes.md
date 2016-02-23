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


## Lecture 4 - January 29th 2016


**Assembly Language**

Assembly is the language of the machine. It's primitive and has no real sophisticated control flow. Instructions are also very restrictive with what exactly you can do.

During this course we'll work with the MIPS instruction set

- It's similar to other architectures developed since the 1980's.
- Used by NEc, Nintendo, Silicon Graphics, Sony, etc..
- 32-bit architecture
  - 32 bit address line
  - data and addresses are 32 bit


The **Instruction Set** is the repertoire of instructions that a computer has the ability to use. Different computers will have different instruction sets (bt many aspects will be in common!)

Early computers had simple instruction sets. Now they can be much more complex. But simple ones do still exist!

**RISC** Philosophy

- Fixed Instruction length
- load-store instruction sets
- limited number of addressing modes
- limited numer of operations

Examples:

- MIPS, Sun SPARC, HP PA-RISC, IBM PowerPC, ...

**CISC** (C is for Complex!). Ex: Intel x86, AMD64

**MIPS (RISC) Design Principles**

- Simplicity Favors regularity
  - Opcode is always the first 6 bits
- Smaller is faster
  - limited number of registers
  - limited number of addressing modes
- Make the common case fast
  - arithmetic operands from register file (load-store)
  - Allow instruction to contain immediate operands
- Good design demands good compromises
  - 3 instruction formats
  
There are 6 different instruction categories for **MIPS-32 ISA**

- Computational
- Load-Store
- Jump and Branch
- Floating Point
- Memory Management
- Special

Registers are places where we can store values for variables in assembly

There are many different registers in MIPS. The register **$r0 always contains the value 0**.
We also have registers  **0-31, HI, LO**, and the **PC** (program counter)

There are 3 types of instruction formats which are all 32 Bits wide

##### R Format

| op | rs | rt | rd | sa | funct |

##### I Format

| op | rs | rt | immediate |

##### J Format

| op | jump target |


### MIPS R-Format Instructions

| op     | rs     | rt     | rd     | sa     | funct  |
| 6 bits | 5 bits | 5 bits | 5 bits | 5 bits | 6 bits |

Instruction fields:

- op - **operation code**
- rs - **first source register**
- rt - **second source register**
- rd - **destination register**
- shamt - **shift amount**
- funct - **function code**

Example C-to-MIPS code:

**C Code**

    f = (g + h) - (i + j);

**Compiled MIPS Code**

    add $t0, $s1, $s2
    add $t1, $s3, $s4
    sub $s0, $t0, $t1

So then how exactly do we translate these statements into binary (1's and 0's) that our processor can understand


Recall that R-Format instructions have the format

| op     | rs     | rt     | rd     | sa     | funct  |
| 6 bits | 5 bits | 5 bits | 5 bits | 5 bits | 6 bits |

What if we have the instruction:

    add $t0, $s1, $s2

We'll translate it like this

| op     | rs     | rt     | rd     | sa     | funct  |
|:------:|:------:|:------:|:------:|:------:|:------:|
| special| $s1    | $s2    | $t0    | 0      | add    |
| 0      | 17     | 18     | 8      | 0      | 32     |
| 000000 | 10001  | 10010  | 01000  | 00000  | 100000 |

Our final instruction will then be:

`00000010001100100100000000100000`

## Lecture 5 - February 2nd 2016

From here we can actually translate these binary instructions into hexadecimal! The conversion is quite easy using the table below:

| 0 | 0000 | 4 | 0100 | 8 | 1000 | c | 1100 |
| 1 | 0001 | 5 | 0101 | 9 | 1001 | d | 1101 |
| 2 | 0010 | 6 | 0110 | a | 1010 | e | 1110 |
| 3 | 0011 | 7 | 0111 | b | 1011 | f | 1111 |

From this we can translate a binary instruction to hexadecimal:

`1110 1100 1010 1000 0110 0100 0010 0000` ---> `eca8 6420`

**MIPS Register File**

Operands of arithmetic instructions must be from a limited number of special locations contained in the datapath's **register file**

The **register file** holds thirty-two 32-bit registers

This file holds two read ports, and one write port.

Registers are:

  - Faster than main memory
  - Can hold variables so that code density improves (since register are named with fewer bits than a memory location)
  - Register addresses are indicated by using `$`.

**MIPS Naming Conventions for Registers**
  
![](/assets/images/comp-arch/MIPS-registers.png)


**Memory Operands**

In MIPS all arithmetic instructions' operands must be loaded into a register before being operated on.

What if a program includes many, many variables (more than you can use in MIPS)?

  - Then we must store variables in memory!
  - Load values from memory into registers before use
  - Store result from register to memory
  
Memory is byte-addressed, meaning that each address identifies an 8-bit byte.

Words in memory are _aligned_. Aligned means that the address is a multiple of 4.

MIPS is what we call **Big Endian**. This means that memory is addressed from left to right, where the beginning of a word is the leftmost position. So in a 4-byte word, the 0th byte is on the left, and the 3rd indexed position is at the rightmost position.

**Registers vs. Memory**

- Registers are much faster to access than memory
- Operating on memory data requires many loads and stores
- The compiler must use register for variables as much as possible
  - Only spill to memory for less frequently used variables.
  
**Accessing Memory with MIPS**
  
MIPS has two basic _data transfer_ instruction for accessing memory
  
- The data transfer instructions must specify:
  - Where in memory to read (load) from or to write (store) - **memory address**
  - Where in the register file to write to (load) or read from (store) (**register destination**)

- The memory address (a 32 bit address) is formed by adding the offset (plus or minues) the contents of the base address register.

- A 16-bit field (offset) meaning access is limited to memory locations within a region of $$\pm 2^{13}$$ words ($$\pm 2^{15}$$ bytes) of the address in the base register.

**Processor - Memory Interconnections**

- Memory can be viewed as a large, single-dimension array, with an address.
- A memory address s an index into the array.

Accessing  memory with `lw` and `sw`.

Now imagine we have the C-code:

~~~
g = h + A[8];
~~~

Imagine `g` is in `$s1`, `h` is in `$s2`, the base Address of `A` is in `$s3`

So because MIPS is addresses words in multiples of 4, then to find the 8th index of the Array, A, with base address `8 * 4 = 32`. Meaning that to load the value `A[8]` we would use the MIPS line:

~~~
lw $t0, 32($s3) # Load word A[8] into memory
~~~

Then to complete the code translation for this we have:

~~~
lw $t0, 32($s3) # Load word A[8] into memory
add $s1, $s2, $t0
~~~


Now what happens if instead of 8 as our Array location, we wish to address the array's index as a variable, say `i`.

In this case, one option is to multiply the variable by 4, then add that value to the Array's base address.

From there we may load the variable.

Example. Assume that the base address of Array A is `$s3` and that the variable `i` is represented by `$s1`.

Then to multiple `$s1` by 4 we can do a bitwise shift to the left for 2 bits.

~~~
sll $s1, $s1, 2
~~~

Then we must add to that the base address of the array

~~~
add $s4, $s3, $s1
~~~

After which we can then access the memory via

~~~
lw $t1, 0($s4)
~~~

## Lecture 6 - February 5th, 2015

### MIPS I format Instructions


| op     | rs     | rt     | constant or address  |
| 6 bits | 5 bits | 5 bits | 16 bits              |

These **I-type** instructions allow for 'immediate' load and store instructions to the register.

- `$rt` is the destination register
- **constant** is a value between $$-2^{15}$$ to $$2^{15} - 1$$
- Address: offset which is added to the base in `$rs`

![CA - I instruction](/assets/images/comp-arch/ca-i-instruction.png)

In these types of commands, a constant is specified in the instructions.

For subtraction, we can actually just use a negative constant. So instead of using `subi`,  you can use `addi` and make the constant negative.

### The Constant Zero

The MIPS register 0, (`$zero`) is always equal to the constant 0. It **cannot be overwritten**.

It is useful for common operations such as moving between registers

- `add $t2, $s1, $zero`

### Loading and Storing Larger (32-bit) Numbers

Due to the format of I-format instructions, it seems such that we cannot immediately load values which are larger than 16 bits into the register. This seems like a limitation, however there is actually a MIPS command which can work around this. It is called `lui` or `load upper immediate`.

This stores the largest 16 bits into the register specified with the `lui` command. You must then use the command `ori` to store the lower 16 bits into the register, Otherwise just executing the `lui` will fill the lower 16 bits with 0.


### Loading and Storing Bytes

- MIPS provides special instructions to move bytes from memory.

~~~
lb $t0, 1($s3) #load byte from memory
sb $t0, 6($s3) #store byte to memory
~~~

![Load and Store Bits](/assets/images/comp-arch/load-store-bits.png)

### Logical Operators

| Operation | C | Java | MIPS |
| Shift Left| `<<`| `<<` | `sll` |
| Shift Right | `>>` |`>>`|`srl`|
| Bitwise AND | `&`|`&` | `and`, `andi`|
| Bitwise OR |`|` | `|`| `or`, `ori`|
| Bitwise NOT | `~`|`~` | `nor`| 

These commands are all useful for extracting and inserting bits within a word.

### Shift Operations

These are "R Format" instructions.

They have the form:

| op | rs| rt | rd | shamt | funct |
|6 bits | 5 bits | 5 bits | 5 bits | 5 bits | 6 bits |

The shift left and right operators (`sll`, `srl`)

## Lecture 7 - February 9th, 2016

### AND Operations

These are useful to mask bits in a word. We can select some bits, or clear others to 0.

Example:

```
and $t0, $t1, $t2
```

There's also an i-format instruction as well

```
andi $t0, $t1, 0xFF00
```


### OR Operations

The are useful to include more bits in a word. It uses a logical OR to determine whether a bit should be included in the result

There are two form (R and I format):

```
or $t0, $t1, $t2
ori $t0, $t1, 0xFF00
```


AND and OR instructions literally check bit by from each word and determine whether the bits should be flipped or not.

### NOT Operations

There's actually no `not` operator that you can execute against a register

_However_, it is possible to use a **NOR** operator

- `a NOR b == NOT (a OR b)`

```
nor $t0, $t1, $zero
```

This will flip each bit to the opposite of what is currently in the word

### Conditional (Control FLow) Operations

In these instructions we will branch (jump) to a labeled instruction or location if a condition is true

We have 3 different types of _control flow_ operations.

- Branch if equal
- Branch if not eqal
- jump to

Examples of these three instruction (you should be able to figure out which correspond to which)


    beq rs rt L1
    bne rs rt L1
    j L1
    

L1 is simply a location where we determine j

### Compiling IF Statements

In C code we might have something like...


    if(i == j)
      f = g + h;


Compiled to MIPS we might see something like:


      bne $s3, $s4, Else
      add $s0, $s1, $s2
      j Exit
    Else: sub $s0, $s1, $s2
    Exit: ...



### Compiling Loop Statements

In C, we might have code which looks something like:

```
while (save[i] == k)
  i += 1;
```

Then, if compiled to MIPS using the control statements it will look something like:

    
    Loop: sll $t1, $23, 2
          add $t1, $t1, $s6
          lw  $t0, 0($t1)
          bne $t0, $s5, Exit
          addi $s3, $s3, 1
          j Loop
    Exit: ...
    

### Conditional Branches

Instructions:

- `bne $s0, $s1, Label`
- `beq $s0, $s1, Label`

These I format instructions have the form 

| op | rs| rt | jump location |
|6 bits | 5 bits | 5 bits | 16 bits |

but how exactly is the 16 bit branch location calculated?

### Specifying Branch Destinations

Use a register added to the 16 bit offset.

- We will use the instruction address register.
  - the PC gets updated during the **fetch** cycle cso that it holds the address of the next instruction
  
- This limits the branch distance of an instruction to be $$-2^{15}$$ or $$2^{15} - 1$$

### Other Conditional Operations

```
slt rd, rs, rt
```

- Set result to 1 if a condition is true, otherwise set 0
- `if (rs < rt), rd = 1;, else rd = 0`

```
slti, rt, rs, constant
```

- `if (rs < constant) rt = 1; else rt = 0;`

These two can be used in combination with beq and bne

Example:


    slt $t1, $s1, $s2 # If ($s1 < $s2)
    bne $t0, $zero, L # Branch to L


## Lecture 6 - February 12th 2016

**Branch Instruction Design**

Why don't we have a `blt` (branch greater then) or `bge`(branch greater or equal)?

 - The reason is that in hardware these types of instructions are physically slower to run than the $$=$$ or $$\neq$$


### Signed vs. Unsigned

- Signed Comparison instructions: `slt`, `slti`
- Unsigned Comparison: `sltu`, `sltui`

Example:

![](/assets/images/comp-arch/signed-unsigned-comparisons.png)

### More Branch Instructions

We can use `slt`, `beq`, `bne` and the fixed value of 0 in register `$zero` to create other conditions

Example: **less than**

    slt $at, $s1, $s2
    bne $at, $zero, Label

In MIPS we have pseudo-instructions which allow us to do this on a single line using `ble`, `bgt`, `bge`


### Jump Instructions

Just instructions come in the form

    j label # Goes to label, 'label'

Machine Format:

| op | 26-bit address |
| 2  | ???? |

But how exactly is the jump destination address specified? 

- An _absolute_ address is formed by concatendating the **upper 4 bits of the current PC** and concatendating `00` as the 2 low-order bits.

**Branching Farther Away**

What happens when a destination address ends up being more than 16 bits away? (the maximum length for a branch location in a `bne` or `beq` command.)

The assembler will attempt to then invert the comparison command and then place a `j` command after the comparison

Example:

      beq $s0, $s1, L1

Will then become

      bne $s0, $s1, L2
      j L1
    L2:
    
## Procedures

Procedures (also known as methods or functions) in C like languages are usually their own blocks of code which run given a set of arguments.

The issue is that given a procedure we must return to a previous spot in our code form thwere is was originally called. It is possible to do this with assembly.

First let's look at the main steps of a procedure

1. The main routine (caller) places actual parameters in a place where the procedure (callee) can access them.
  - Typically registers `$a0 - $$a3`
2. The caller transfers control to the callee
3. The callee axquires the storage resources needed
4. The callee performs the desired task
5. The callee places the result value in a place where the caller can access it
 - `$V0` and `$v` are typically the values for return registers
6. The callee then returns the control to the caller
 - `$ra` is the return address register to return to the point of origin.


The format for doing this in MIPS assembly is:

    jal procedureAddress # jump and link
    
This saves PC+4 in the register `$ra` and then jumps to the address `procedureAddress`

The callee will then do the procedure return with just 

    jr $ra

### MIPS Register Conventions

![](/assets/images/comp-arch/mips-register-convention.png)


### Spilling registers

But what will happen if the callee needs to use more registers than allocated to argument and return values?

- The **callee will use a stack** a LIFO queue

One of the general registers `$sp` (also known as `$29`) is used to address the stack which 'grows' from high address to low address

- **Add data onto the Stack** ("push")
  - `$sp = $sp - 4`

- **Remove Data from the Stack** ("pop")
  - `$sp = $sp + 4`
  
**So what does this mean for us**? How can we write a method which uses recursion?

Or in other words **how do we code in assembly, a situation where **

Basically what we need to do is write to (and pull from) the main memory using the **stack pointer** `$sp`

We see when using `jal` and `j $ra` the PC (Program Counter) is stored to whichever address is currently present in the `$sp` register.

But if we know that we need to call a recursive function we need to do three separate things:

1. Store all of the register values that we need once we get out of the recursive call
2. Make the resursive call
3. Restore the stack pointer counter to its original position

One thing that should be noted is that **the stack pointer starts at high memory addresses and progresses to lower ones**

So if we want to save two different register values `$t0` and `$t1`, with our stack `$sp`, we would write the following

	addi $sp, $sp, -12 # Make 3 spaces for our values
	sw $ra, 8($sp) # Store return address sp - 4
	sw $t0, 4($sp) # Store a variable at sp - 8
	sw $t1, 0($sp) # Store a variable at sp - 12

After looking at this code we can see that we first add `-12` to our `$sp`

This creates space for $$\frac{12}{4} = 3$$ different register values.

However the original stack pointer `$sp` should not be modified. This why we store our old `$ra` with the original stack pointer -4. `$sp + 12 - 8 = $ra`

Then the same is done for `$t0` and `$t1`

So then what do we do once we get out of the method call? We must return our stored values from the stack

	lw $t1, 0($sp) # Put $sp - 12 back to $t1
	lw $t0, 4($sp) # Put $sp - 8 back to $t0
	lw $ra, 8($sp) # Assign return address original value
	addi $sp, $sp, 12 # Return stack pointer to original location
		
So then if we wanted to generalize the process of performing a method call

1. Allocate $$4\cdot(n+1)$$ new memory locationd to utilize on the stack pointer where $$n$$ is the number of register values that need to be stored. (Use the `addi` instruction)
2. For each register value which must be stored, use the `sw` command to store each word at a unique `$sp` location which ranges from `$sp` - 4(n+1) to `$sp` - 4 (inclusive). Remember to store the `$ra` value!
3. Then we can call the method using `jal`
4. After returning from the method, restore the register values by using the `lw` instruction to restore the register values from memory.
5. Continue method execution

**Table of MIPS instructions**

![](/assets/images/comp-arch/mips-instruction-table.png)


### Byte and Halfword Operations

These instruction can be useful to load values for bitwise operations. In addition to the load byte `lb` command which will load a single byte from memory, there is also a `lh` command which stands for **Load Halfword** which will load only half of a byte.

- Sign extend to 32 bits
  - `lb`, `lh`
- Zero extend to 32 bits
  - `lbu, lhu`
- Store just the rightmost byte or halfword
  - `sb`, `sh`

### MIPS Addressing Modes

- **Register addressing** - operand is in a register
- **Base (displacement) Addressing** - The operand is at the memory location whose address is the sum of a register and a 16-bit constant contained within the instruction
- **Immediate Addressing** - The oeprand is a 16-bit constant contained within the instruction
- **PC Relative Addressing** - Instruction address is the sum of the PC and a 16-bit constant contained within the instruction
- **Pseudo-direct Addressing** - The instruction address is the 26-bit constant contained within the instruction concatenated with the upper 4 bits of the PC

### Compilers

A compiler transforms programs from a high level language (like C!) into an assembly language program.

Advantages of high-level languages

- fewer lines of code
- Much much easier to understand and debug

Many compilers today will optimize high-level code and produce efficient assembly code which is nearly as good as an assembly language expert

### Assembler

- Transforms symbolic assembler code in object (machine) code

Advantages of assembly language

- Programmer has much more control than higher level languages
- Much easier than remember binary instruction codes
- Can use labels for addresses and let the assembler do arithmetic
- Can use pseudo instructions

However, we must remember tht machine language is the underlying reality

Also considering performance we should still count the instructions executed, not the size of the code.

### Other tasks of the Assembler

- Determines the binary addresses corresponding to all labels
  - keeps track of the labels used in branches and data transfer instructions in a _symbol table_
- Converts pseudo-instructions to legal assembly code.
  - register `$at` is reserved for the assembler to do this.
- Converts branches to far locations into a branch followed by a jump
- Converts instructions with large immediates into a load upper immediate followed by an or immediate
- Converts numbers specified in decimal and hexadecimal into their binary equivalents
- Converts characters into their ASCII equivalents

### Memory Layout

![](/assets/images/comp-arch/memory-layout.png)

### Producing an Object Module

- An Assembler (or compiler) translate a program into machine instrucitons.
- It provides information for building a complete program from the pieces.
  - Header: described object module content
  - Text segment: translated instructions
  - Static data segment: data allocated for life of the program
  - Relocation info: For contents that depend on absolute location of loaded program
  - Symbol table: glocal definitions and external refs
  - Debug info: for associating with source code
  
### Linker
  
- A linker takes all of the independently assembled code and stitches (links) them together
  - It is much faster to patch code and recompile and reassemble the patched routine than it is to recompile and reassemble the entire program
- The linker decides on the memory allocation pattern for the code and data modules of each segment
  - Remember, segments are assembled in isolation, so each assumes the codes starting and data location are the same.
- Absolute addresses must be relocated to reflect the new starting location of each code and data module
- The linker uses the symbol table information to resolve remaining undefined labels.

### Loader

- The loader will load (copy) the executable code which is stored on the disk into memory at the starting address specified by the _operating system_
- This initializes the machine registers and sets the stack pointer to the first free memory location
- It will copy any parameters to the main routine and onto the stack
- Finally it jumps to a start-up routine that copies the parameters into the argument registers and then calls the main routine of the program with `jal main`

------------------------------

## Arithmetic With Computers

The first thing that we need to understand is two's complement.

It is a way of representing positive and negative binary numbers.

Example: Imagine we have four bits `0000`. Say we want to represent the value
`-5` using the two's complement.

1. Represent `-5` in binary.
  - $$2^0 + 2^2 = 0101$$
2. Note `0101`, now invert every bit to its opposite value
  - `0101` inverted `1010`.
3. Add 1 to the inverted value
  - `1010` + `0001` = `1011`
  
`1011` is the two's complement representation of the number -5

And what about going backwards (negative to positive)?

Simply subtract 1 from the value, then invert the bits. The exact opposite process from before.

### Two's Complement Operations

MIPS 16-bit arithmetic gets converted to 32 bit for arithmetic

- Digits are **sign-extended** - We copy the most significant bit (the sign bit) into all empty bits

- Sign extend vs Zero Extend (`lb` vs `lbu`)

In binary if we wanted to add two single bits lets say `1` and `1`, then to do this we would need to












































