---
layout: page
title: CSE 221 - Operating Systems - Notes on "The UNIX Time-Sharing System"
permalink: /education/grad/cse221/10-15/unix/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, rust, memory, OS, unix
description: Notes for "The UNIX Time-Sharing System"
mathjax: true
---

> Q: What aspects of Unix as described in the 1974 paper do not survive today,
> or have been considerably changed?


- Powerful OS at low cost
- Significantly easier to use than most
- OS includes assembler, text editor, linking loader, debugger, comopiler,
  interpreter of BASIC, text formatting, fortran, macro processor

- built on dogfooding
- minimum 50kb of core memory
- written in assembly, then re-written in C.
- Lots of device drivers

- most important role is to provide the FS
- contains
    - disk files
    - directories
    - special files

- Ordinary files
    - could be data, programs, etc
- directories induce a tree structure
    - enforce a limitation on file name length
    - a file which is not a directory may appear in many places (linking)
        - even with different names
    - has `.` and `..` concepts for directory traversal
- special files
    - every IO device corresponds to a file found under `/dev`
    - useful b/c file and device IO make interaction easy
    - same protection mechanisms as files

Removable FS

-  include a mount system
    - provide a file name, and a device which has another FS on it.
    - no links between FS


Protection

- Users get UIDs
- 7 protection bits, but only for owner, and all other users (no group)
    - 7th bit is for change of UID on program execution.
- set-user-id bit is used to cross execution boundaries (i.e. for mkdir)

IO Calls
- no predetermination of IO size
- no distinction on "random" or "sequential" IO
- open with `filep = open(name, flag)`
- operate on file with `read`, `write`, and `seek`

Implementation of the FS

- directory gets the i-number corresponding from its table of children ->
  i-number (inode id)
- looks up i-number in system table to read from FS location
- FS location of a file contains
    - owner, protection bits, addresses of blocks, size, last mod time, number
      of links to file, dir bit, special file bit, small/large bit
- file descriptor corresponds to table mapping of read/write pointer, device,
  and inumber
- creation corresponds to adding a direcory link, increasing inode link count by
  1
- deletion corresponds ton decremting link count, removing dir entry, and then
  erasing blocks in the inode's link count is 0
- small/large file bit determines whether the file blocka ddress are stored
  directly, or a layer of indirection is added to store more block addresses for
  additional file capacity (1M)
- special file handling
    - if the inode is special, the last 7 device addr words don't mattr. The first word is a pair of bytes identifying and internal device name.
        - two bytes specify device type and device number.
        - device type indicates which routine handles IO on the device, and subdevice number selects the device attached to the controller of one or more connected interfaces
- write IO, page is read in if necessary, and byte replaced - actual write make take place at a later time

Processes and Images

- image (program) structure divided into 3 parts
    - 0 to N program text (data)
    - N + (8k / N) (next 8k byte boundary) to M is shared writeable data segment (heap)
    - Max stack to M is the stack.
- new process only via `fork()`
- communicate with `pipe`
- execute another program with `execute`
- synchronize on `wait()`
- exit with `exit()`

The Shell

- command line interpreter
- use `>` to output command output to a file
- use `<` to read from a file data into a program
- piping output with `|`
- commands one after another with `;`
- `&` to run in background
- shell can call itself
- has `init` program
- interrupt a program from `delete` character.
- quit signal can be invoked, dump core image.




