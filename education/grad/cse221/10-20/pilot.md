---
layout: page
title: CSE 221 - Operating Systems - Notes on "Pilot; An Operating System for a Personal Computer"
permalink: /education/grad/cse221/10-20/pilot/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, rust, memory, OS, pilot, pc, personal, computer
description: Notes for "Pilot; An Operating System for a Personal Computer"
mathjax: true
---


> Q: How do the requirements of the Pilot operating system differ from the
> systems we have read about so far, and how does the design of Pilot reflect
> those differences?

**Answer**:


### Introduction

- system built for hardware which, "a few years ago" (1980), could have been
  used as a time-sharing system
  - built on mesa PL
- single-language, single-user design
- limited protection and resource allocation
    - defensive rather than absolute
    - depends on type checking in Mesa

- not oriented toward fair distribution of resources
- pilot is _basically_ a runtime for Mesa

### Pilot Interfaces

- two module types
    - **programs** containing implementations
    - **definitions** specify interfaces between programs
- kind of like libraries which define headers (`.h`) and impl. (`.c`)


- makes distinction between interface and implementation
    - Two types of interfaces
        - **Public** interface define services provided by pilot to clients
        - **private interfaces** form connective tissue binding implementations
          together

#### Files

- **File** interface
    - represent media on which files are stored
- **Volume** interface
    - files are containers for information storage
- no directory hierarchy
- file size adjustable by units of pages
    - accessed by mapping a page to virtual memory
- `File.Create` capability
    - files and volumes named by a 64-bit universal identifier (uid)
- removable volumes expected for information transfer
- uid
    - concat machine uid w/ 32-bit timestamp
- attributes
    - size
        - 512 bytes/page $$2^{23}$$ pages
        - type: 16 bit tag
        - permanence: temp cleaned up after failure, permanent files are not
        - immutability: permanently read-only: good for replication without
          worrying about damaging contents
- Different types of hardware are supported
- volume "logical", does not represent a single piece of hardware
- can have many volumes on a single physical disk


#### Virtual Memory

- $$2^{32}$$ physical addresses (each addresses 16 bits, 2 bytes)
- all computations in the same address space...hmmm
- each page has _referenced_, _written_, and _write-protected_ flags
- Address space structures as _Spaces_
- Spaces have three fundamental roles
    - _Allocation Entity_: allocates regions of VM
    - _Mapping Entity_: associates content and backing store with region of VM.
    - _Swapping Entity_: transfers pages between primary memory and backing
      store
    - Spaces many have any or all of the above roles
- Access files with memory mapping via `Space.Map`
    - page can only be accessed if exactly one of the nested spaces containing
      the page is mapped
- on _addressFault_ or _WriteProtectFault_ debugger is activated automatically
- demand paging is not default
- `Space.Activate`, `Space.Deactivate` and `Space.Kill` to let OS know that a
  space will be used, not used, or is of no further interest
    - prevent useless swapping
- files can only be accessed through VM
- Mapping better
    - separate map and swap decouples buffer allocation and disk scheduling
    - read-write privileges of a file capability can propagata automatically to the space in hardware
    - advice-taking operations can inform system about program behavior to eliminate inefficient operations (swap)
    - easy to simulate a r/w interface with a mapping interface and appropriate use of advice
- `Space.ForceOut` to force write to backing file

#### Streams and IO

- access IO devices
    - implicit (pilot file via VM)
    - direct: low-level device driver interface exported from Pilot
    - indirect: pilot stream facility
- streams facility has the `Streams` interface
- stream components for acting on data in the stream
- cascading stream components to form compound streams
- _transducer_ imports device driver and exports a stream
- _filter_ imports an instance of a stream and exports it to another
- create pipelines
- transducer is closest to the hardware

#### Communication

- Shared-memory communication
- tightly coupled processes vs loosely coupled
- internets: interconnection of networks
- ethernet: local high-bandwidth network
- internetwork routers (gateways): connect networks
- identify via addresses
- optimize transfer of data if addressing local computer
- sockets transmit packets
- network address
    - 16 bit network number
    - 32-bit processor ID
    - 16 bit socket number
- Three levels
    - L0: every packet must be encapsulated for transmission
    - L1: defines format of network packet, incl. source and destination
    - L2: packet types; e.g. error, connection sequence, routing table update,
      etc
- Socket provides L1 access
- sockets also via streams
- no guarantee on reliability of medium, only that it is sent
- Servers and clients
- simpler than TCP

##### Mesa Language Support

- Pilot provides runtime support
    - explicit by mesa interfaces exported (e.g. _Process_)
    - implicit: compiler-generated calls to built-in procedures
    - via traps: machine-level op-codes during exceptions
- Implements a heap
- world-swap debugging
    - save state, write a boot file, boot into debugger virtually no change to
      program

### Implementation

##### Storage System

- kernel/manager implementation
- functional hierarchy where system components are layered and build off one
  another
- expect 3 types of load
    - short period with static working sets, fs not needed
    - client working set changes slowly
    - client working set changes drastically w/ heavy swapping
    - system expected to behave in all situations

#### Cached DBs of the VM Implementation

- vm manager implements client visible space operations
- maintains vb checking validity of backing representation of Spaces
- swapper maintains the space cache which is loaded from the hierarchy
- Other structures
    - bit table on pages of memory
    - swap units, the smallest set of pages that can be swapped
- swap unit cache indexed by page to find the space where a page fault occurred
- makes some key records in the swap unit/page cache ineligible to swap

#### Process Implementation

- implementing concurrency facilities split among places
- basic primitives: monitor, forking, condition var
- Pilot is a collection of monitors provided via Mesa, and group of system
  processes

#### FS Robustness

- files are self-identifying
- damages to hardware only affect the damages portions data
- primary structure on volume is a b-tree keyed on file-uid and page-number to
  device address of page
- allocation map of page-allocated

#### Communication Implementation

- implemented in-kernel, packets must be buffered, examined, then routed (or
  dropped)