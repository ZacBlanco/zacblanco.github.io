---
layout: page
title: CSE 240A - Prin. Programming Languages
permalink: /education/grad/cse240a/
keywords: computer architecture, cpu, pipeline, instructions, UCSD, UC, San Diego, CSE, 240A, intel, amd, flop
description: Notes for CSE CSE 240A (Computer Architecture) at UC San Diego
mathjax: true
---

## Lecture 1

- Two types of "time"
  - latency (time to complete a single tasks)
  - throughput (time to complete many tasks)

- How to calculate **speedup**
  - Speedup of A over B
    - Latency: $$ B / A $$ (divide faster term)
    - Throughput: $$ A / B $$ (divide slower term)
  - Speedup % of A versus B
    - Latency: $$ 100 * ((B / A) - 1)) $$
    - Throughput: $$ 100 * ((A / B) - 1) $$

- Percent increase/decrease are not the same
  - % increase -- use smaller number as baseline
  - % decrease -- use larger number as baseline

- Use arithmetic mean for the latency (proportional to time) ($$ \frac{1}{N} \times \Sigma n $$)
- harmonic mean for throughput (not proportional to time) ($$N / \Sigma \frac{1}{n}$$)
- geometric mean for speedup

- **CPI** -- Cycles per instruction (1/IPC)
- **IPC** -- Instructions per cycle (1/CPI)
- **Clock, Hz, Frequency**: seconds per cycle

## Lecture 2

More performance metrics:

- MIPS: millions of instructions per second
- FLOPS: floating-point operations per second

- Ahmdahl's Law: $$ \text{total speedup} = \frac{1}{(1 - P) + \frac{P}{S}} $$
  - P: proportion of prog affected by optimization
  - S: speedup of particular portion

- Little's law: $$ L = \lambda W $$
  - L = items in the system
  - $ \lambda $ = average arrival rate
  - W = average wait time

- Performance, Workloads, and benchmarks
  - Workload: set of tasks to run
  - Benchmark: standard workloads
  - microbenchmarks: non-standard workloads

### ISAs

- specification of a set of opcodes and native commands implemented by a
  processor
  - contract b/t hw and sw
  - functional definition of storage locations, operations

- **atomic** -- instructions finish before next executes
- ISA Design
  - need to express programs efficiently, low power, cost, etc
  - most of the time, ideally somewhat compatible with older ISAs
  - CISC/RISC
    - CISC: complex instruction set: heavyweight/complex instructions, high CPI
    - RISC: simplest insn only, simple insns, low CPI


## Lecture 3 - ISAs

- ISA review
  - ISA is contract between HW and software
  - basically the API definition for hardware
  - defines:
    - functional definition of storage locations, operations
    - precise description of how to invoke ops+access data
- stages of execution
  - fetch, decode, read inputs, execute, write output, next insn

- CISC: variable length instructions (generally 1-321 bytes)
  - 14 regs, PC, SP, condition codes
  - data sizing of 8, 16, 32, 64, 128
  - x86 has insn lengths of 1-15 bytes
- RISC: MIPS, PA-RISC, SPARC, ARM, RISC-V
  - 32-bit insn
  - 32 int regs, 32 FP
  - load/store arch

| RISC | CISC |
| single-cycle execution | many multicycle ops |
| hardwired (simple) control | microcode for multi-cycle ops |
| load/store arch | register-memory + memory-memory |
| few memory addressing modes | many modes
| fixed-length insn format | many formats + lengths |
| reliance on compiler optimizations | handle assemble for good perf |
| many registers | few registers |

- supposedly, intel uses RISC-like ops inside its CPUs to implement more complex
  insns.

### Aspects of ISAs

- length of instructions
  - use fixed or variable length
  - variable length saved mem/space
  - compromise: use only 1 or 2 lengths
- formats
  - e.g. opcodes and argument placement
- ops and datatypes
  - integers, flops (IEE754 FP)
  - more recently CPUs support
    - packed int (MMX)
    - packed FP (SSE/SSE2/AVX)

- data lives in registers
  - faster than memory, named directly in insns
  - otherwise, access memory to put daa in registers
    - slower, but much larger amount of memory
    - addressed depending on addressing mode
  - immediate values also specify values directly in insns (e.g. 17)
- trends have been to add more registers
- memory addressing modes in ISA
  - displacement `R + imm`
  - index-base: `R1 + R2`
  - memory-indirect: `mem[R]`
  - auto-increment: `address = R, R = R+1`
  - auto-indexing: `address = R + imm, R = R + imm`
  - scaled: `addr = R1 + R2*imm + imm2`
  - PC-relative: `addr = PC + imm`

- computing next insn target:
  - can be PC-relative: `bne R1 R2 L3`
  - absolute: `jmp: L3`
  - register indirect: `jmp R`

- ISA design principlies
  - make the common case fast
    - e.g. byte addressability
    - caches/memory are usually aligned to some byte offset
    - aligning addresses within maximum size can ensure better perf
    - ways to handle:
      - completely disallow
      - allow (adds hw complexity)
      - trap to SW routine

## Lecture 4 - More ISAs and Pipelining

- How to design high-performance ISA
  - make the common case fast
  - make the fast case common

- operand models: register or mem
  - load/store arch (mem accesses are distinct)
  - mixed-model: ops can be mem or register

 ### pipelining

 - instructions can overlap in the CPU while running
 - pipelining the datapath:
  - latency/throughput of insns
  - single-cycle instruction: no need for pipelining, one insn/cycle
  - utilize data dependencies to simply execute instructions in overlapping
    fashion

# Lecture 5 - Pipelining cont'd

- Write into tables:
  - F, D, X, M, W
    - Fetch, Decode, Execute, M, Write

- Dependencies:
  - relationship between instructions
  - data: two instructions use same storage location
  - control: one instruction affects whether another even executed
  - must enforce dependencies properly
- hazard: dependence and possibility of wrong instruction order
  - stall: waiting to keep later instructions properly executing
  - stalls+hazards: bad
- structural hazard: when writing pipeline table, cannot have two insns in same
  pipeline stage at the same time

- solving data hazards w/o stalls: Bypassing
  - send result of execution stage directly back to the previous
    stage(s) (MX bypassing)
  - WX bypassing: add a bypass and mux directly from write stage
  - WM bypass: WM bypass goes from write to M stage input

- multi-cycle operations: e.g. 4-cycle multiply
  - P: separate output latch connects to W stage
  - multiplication is often pipelined in MIPS machines

## Lecture 6 - Pipelining and Branch Prediction

- on control instructions  (e.g. `bnez`) in order to execute the next
  instructionthe branching instruction needs to write its result in the
  M stage in order to continue execution
- to reduce the cycle penalty, attempt to guess the next branch instruction
  - however, might get a penalty if we guess wrong
- solution: add a mux with a selector that tells the pipeline stage to accept
  (or not) the result of the instruction from the current cycle
- on a simple processor, only two insns begin exeuction before we know branch
  result, so need only 2 muxes in order to throw away result based on mux result
- when to perform branch prediction?
  - look at instruction opcode to determine if branch
    - calculate next PC from instruction
    - one cycle mis-fetch penalty even if BP is correct
  - determine if branch during the fetch (rather than decode)
    - save one cycle by fetchin
- components of branch predictor
  - BTB: Branch target buffer
    - predicted target insn location
  - BHT: Branch history table
    - taken/not-taken results
  - RAS: return address stack
  - 3 steps:
    - is it branch insn?
    - is the branch taken or not?
    - if branch taken, where to go? possibly modify BTB/RAS
- BHT
  - simplest direction predictor
  - PC indexes table of bits (0 = no/false, 1 = yes/true)
  - most basic impl: assume branch goes same direction as last branch
  - example where this is good: for loop (`for (int i = 0; i < 100; i++)`)
- BTB
  - determine where to jump based on insn
  - simplest impl: table with address tag lookup and a predicted PC after branch
  - store last jump location with address tag
  - as a logic flow chart
    - Fetch: send PC to memory and BTB
     - in BTB?
      - Decode: no: last branch taken?
        - execute: no: normal execution
        - execute: yes: enter branch IO and next PC into BTB
      - decode: yes: send predicted PC, is the branch taken?
        - execute: no: mispredicted branch, flush pipeline and delete BTB entry
        - execute: yes: prediction correct

## Lecture 7

- correlated branch predictors
  - use correlation instead of history to predict branches
  - (m, n) predictor
    - m of correlation
    - n bits of predictor for branch
    - last m branch each with an n-bit predictor
    - implementation: global history with selected address bits (gselect)
      - m-bit registoer holds the outcome of the last m branches
      - BHT indexed via m:low(PC)
  - history table of last N taken branches
    - look at last N to lookup which outcome to take
    - update state table with true outcome

- design choice of branch predictor
  - global correlation or local correlation?
    - e.g. nested conditionals in a large loop, might be uncorrelated with one
      another
- design choice: how many bits on the branch history register?
  - larger BHRs generally give better results but are more expensive and take
    more space
  - BHT utilization can be low (many patterns not seen)
  - more bits -> more training time
  - typically 8-12 bits

- how to balance local vs global branch prediction
  - type 1:
    - first, search history table, indexed by PC
    - next, two-bit predicted by history from above table
  - type 2:
    - gshare - similar to gselect: branch address and global history are
      combined with XOR instead

- get better - combine the two (tournament predictor)!
  - use both predictors and use a selector to determine the type of prediction
    to actually use

- perceptron predictor (Jimenez)
  - history table replaced by table of function coefficients
  - predict taken is the sum of the BHR*weights is greater than a threshold


### Hardware Multithreading

- advantages
  - no need to check dependencies between threads (most of the time)
  - no need for predictor logic
  - stalled cycles now used to execute instr.
- disadvantages
  - extra hardware complexity, contexts (PC, regs, etc)
  - reduced single-thread perf
  - resource contention between threads
  - requires some dependency checking

- why use hw multithreading?
  - single-thread pipline utilzation is < 50%
  - use pipeline as much as possible

- 3 types
  - coarse-grain MT
  - fine-grained MT
  - SMT

## Lecture 8 - Out of order execution

- static vs dynamic scheduling
  - static: software based instruction scheduling (schedule before starting to run)
  - dynamic: schedule instructions at runtime

- instr scheduling determines the order of instructions that get executed in the
  pipeline
- execution order does not need to match the order written in software
- HW has knowledge of dynamic events on very fine level
  - branch mispredictions
  - load/store addr
  - cache misses

- data dependencies prevents dispatch of later instructions into execution units
- load and store instructions may take multiple cycle in the writeback stage
- out of order execution:
  - set of insns fetched and put into OoOE buffer
  - insns dispatched OoO
  - reordered in a re-order buffer before writeback

## Lecture 9 - OoO Execution Cont'd

- Enabling OoO Execution
  - must link consumers of values to a producer
    - this is called "register renaming" -- linking a tag with each value
  - must buffer instructions until ready to execute
  - register renaming invented by robert tomasulo in 1960s
- reservation station (issue queue)
  - implemented with RAM or CAM

- some dependencies are not true dependencies
  - same registers refers to values that have nothing to do with one another
    - exist because there are not enough register in the ISA
- different types of dependencies
  - Read-After-Write (RAW) -- real dependency
  - Write-After-Read (WAR) -- not a real dependency
    - instructions must still be ordered properly
  - Write-After-Write (WAW) -- not a real dependency
    - instructions must still be ordered properly
- how to handle?
  - register IDs are renamed to the reservation station that holds the
    register's value
  - after renaming, reservation station entry ID is used to refer to the
    register
      - use a map table from ISA register to "physical" registers
  - when "committing" an instruction move old physical register back to
    free list
  -


## Lecture 10 -- Memory Heirarchy

- ideal memory functionality
  - mem operate of processor speed
  - large enough for all programs
  - cost-effective
- no hardware achieves all of these
- memory hardware is slow
   - 3GHz processor executes an ADD in < 1ns
   - single memory access is >= 33ns

- types of mem
  - SRAM (static RAM)
    - requires more transistors, optimized for speed
  - DRAM (dynamic RAM)
    - single transistor+capacitor req'd
    - slower access (30-50ns)
    - completely different fabriation steps
- over time, CPUs have gotten much faster, but memory has not
- the hierarchy
  - register file (< 1ns)
  - L1 cache (SRAM, ~1ns)
  - L2 cache (SRAM, many ns)
  - L3 cache (SRAM, )
  - main memory (DRAM, ~100ns)
  - swap (disk/flash, 1000s of ns)

- localities
  - temporal locality
    - programs do the same thing over and over again
  - spatial locality
    - programs work with things nearby one another in memory
- cache design
  - bit for valid/not valid
  - tag
  - data (generally 64B)
  - cache capacity measured in stored _data_

- how to use the cache?
  - how to know if item is in the cache
  - if it is, how do we find it?
- modern cache
  - `address --> tag store --> data store`
- assuming a 32-bit cache
  - 2 (leftmost) bits are used to index the byte in the cacheline to retrieve
  - 2 (2nd from leftmost) bits are used to index and determine which cache block

```
+---------------+-----------------+-------------+
| Tag (n bytes) | Index (n bytes) | byte offset |
+---------------+-----------------+-------------+
|   Block Address                 |             |
+---------------+-----------------+-------------+
```

- byte offset: calculated based on the cache block size (`log2(cache_block_size)`)
- index: calculated based on total number of cache blocks (`log2(number_cache_blocks)`)
- tag: the rest of the bytes in the address


- calculating the tag overhead
  - data cache is N bytes
  - tag+valid bytes are K
  - overhead --> K / N

## Lecture 11 - Caches cont'd

- cache is accessed during F and M stages (fetch and memory stage)
- how to calculate AMAT with multiple levels of caches
  - `AMAT = t_hit + (%miss * t_cache_penalty)`
  - `t_avg = t_L1_hit + (%_L1_miss * (t_L2_hit + (%_L2_miss * ...)))`

- inclusive vs exclusive caches
  - e.g. exclusive, data stored in L1, L2, L3, but never in multiple levels
  - inclusive cache
    - data in L1 cache must be in higher cache level
  - non-inclusive cache
    - does not require L1 cache data to be in higher level caches, but can be.

- direct vs associative mapped caches
  - direct mapped cache identifies single cache location
  - set associative can access two or more multiple cache entries at once
  - fully-associative cache can put a main memory block into any cache location

- for fixed size cache
  - increase factor of two in associativity
    - doubles number of blocks per set
    - halves the number of sets
    - decrease size of the index by 1 bit, increase size of the tag by 1

- handling hits/missed
  - read hits, just read the cache
  - write-hits
    - when to propagate to next-level in the hierarchy?
    - option 1: write-through: consistent across the hierarchy
      - update cache and immediately send write to higher levels
      - forces consistent values
    - option 2: write-back
      - allows inconsistency across caches and main memory
      - write data only into the cache block
      - write back contents to higher level when cache value is evicted
      - requires additional "dirty" bit per block

- for write-back, need a Write-Back Buffer (WBB) to hold entries which need to
  be written to higher levels in the memory hierarchy
  - how big to make WBB?
  - Little's law: `L = \lambda W`
  - `L = % miss L1 * latency L2`

- handling cache misses
  - read cannot continue without data -- program must stall
  - write miss, no instruction need to wait, then no need to stall?
- add an additional buffer, the Store Buffer (SB) to hide the latency of writes
  - stores go into the SB and then continue
  - SB writes data in background
  - reads now must search the SB as well
  - eliminates stalls, but creates issues in multiprocessors


- cache replacement policies
  - direct-mapped caches have only 1 option
  - set associative may have a few
  - policy options: LRU, Random, FIFO, NMRU, Belady's
- cache miss model
  - compulsory miss
    - cache block was entirely empty
    - can't make this any better
  - capacity miss:
    - cache too full, must evict on read
  - conflict
    - miss caused because cache associativity is too low
  - coherence
    - miss due to external invalidations (multiprocessors)

- simplest way to reduce miss chance is increase the capacity
- another option: increase the block size to exploit spatial locality
  - block size has a sweet spot for decreasing miss rate
  - U-shaped graph
- miss rates on instructions and on data
  - two separate statistics
- prefetching -- speculatively choosing cache blocks to pull into cache before
  it's accessed.

## Lecture 12 - Main Memory

- main memory consists of DIMMs
  - Dual In-line Memory Module
  - DIMMs connected to a single channel shares a data bus
- memory chips on a DIMM are broken into two ranks (generally one rank on each
  side of the DIMM)
- rank broken down into chips
  - single transfer is 64 bits (8 bytes)
- each chip is broke into banks
  - banks are connected to a mux
- banks are independent memory arrays inside of a single DRAM chip
  - bank is 2D array, organized as rows and columns
  - single byte width -- minimum access size
- hierarchy
  - channel
  - DIMM
  - rank
  - chip
  - bank
  - row/column

- the memory controller
  - memory controller: read/write request queues to schedule requests across the
    DDR bus
- memory bandwidth
  - peak BW: frequency * bus width
  - MT/: million transfers/s
  - e.g. DDR4-3200, 128-bit
    - 3200MT/s * 16 byte = 51.2GiB/s

- page-mode DRAM
  - also called a DRAM row
- closed vs open-row DRAM
  - closed row are not buffered in the row after an access
  - open row are buffered in the row buffer after access
  - may only have one open row at a time
- open vs closed row
  - access an "open row" -- data exists in the row buffer already
    - read/write commands reads and writes to the row buffer
  - closed row -- data does not exist in the row buffer
    - activate: command opens row
    - read/write command reads/writes column in the row buffer
    - precharge command closes the row and prepared bank for the next access
    

