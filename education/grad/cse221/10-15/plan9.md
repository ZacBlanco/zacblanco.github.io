---
layout: page
title: CSE 221 - Operating Systems - Notes on "Plan 9 From Bell Labs"
permalink: /education/grad/cse221/10-15/plan9/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, rust, memory, OS, unix, plan9, p9, att, bell, labs
description: Notes for "Plan 9 From Bell Labs"
mathjax: true
---

plan 9

- published 1995
- mainframes to PCs
- weaknesses of UNIX
    - graphics and net not well integrated
    - machines not as seamless
- ibm wanted centrally administered + cost effective.
- time sharing w/ workstations.
- posit UNIX FS was key.
- network protocol 9p, user brings env anywhere.
- backcompat for UNIX
- wanted to build new peripherals
- redistribute work


design

- 3 principles
    - resources are named in FS
    - standard protocol, 9p
    - disjoint hierarchies by different services, joined to together on
    same namespace
- Each user gets a dedicated "view" of the system, not the whole system.
- user namespace customizes view of resources
    - no global view for users.
    - i.e. `/dev/cons` always represents the users console device.
- 9p is structured as transactions between server and client
    - namespace is held by the device, not on server.
- export many resources

command-level view

- machine with screen running window system
- windows in other namespaces
    - may be deleted
- each window has stdin and stdout connected to the window
- window "server" runs on the local term
- window system and text editor on the terminal, intensive apps on the
server

The file servers

- central file server stores perm files and presents them to network.
- single tree, many disks.
- 3 storage levels
    - backup with WORM
- takes days to fully recover a backup
- permissions in dump are same as active FS
    - tech is creating storage faster than can be used

more unusual file servers

- user level processes provide file-like access to devices - 8.5 window
system is unique 2 interface
- one to terminal user, one to computer
- Access to mouse, keyboard, console through `/dev/` - private files for
each
- use FTP FS on `/n/ftp`
- `exportfs` server to expose user namespace to others.
    - e.g. mount `/net` and `/proc`

Configurability and administration

- uniform interconnection makes a flexible system
    - single computer system, or many room system.
- os on many hardware
- reject use of each person having own workstation with private files
- machines use same db for network connections
- easy to upgrade single components, everyone gets benefits

C Programming

- many languages (C, Alef, shell, rc)
- system libraries all have headers to export functions
- have `libc.h`
- c compilers only accept a subset of ANSI directives
- `#ifdef` only in low-level routines

Portability

- portable across architectures
- send data across as streams
- text mostly, complex is binary.
- prefix extensions for architecture to build multiple at same time.

Parallel Program

- `rfork` for kernel threads.
- share memory with a flag on `rfork`, except for stacks
- attach memory with `segattach` syscall
- `rendezvous` to synchronize
- spin locks by arch dependent library.
- separate process for IO

Namespace Implementation

- `mount`, `bind`, `unmount` to part of tree`
- `mount` creates new hierarchy
- `bind` duplicates some existing namespace at another point in namespace
- `unmount` removes
- 9p provides 17 message including means to authenticate, navigate fids, copy
fids, perform io, change file attrs, create, and delete files.
- 9p has rpc over pipe and procedure within kernel.
- mount driver multiplexes RPCs on the same channel with a tag
- mount table sotres list of bindings between connection channels

File Caching

- no explicit support for file caching.
- memory of central sever is shared cache
- version field of file is changed whenever there is an update.
- compare version field `qid` when updating.

Net and Comms

- write messages over TCP by writing to files
- `/net/tcp` with directory for each connection
- write to the `data` file once connection is established
- configured as streams of data
- 9p must run on reliable transport with delimited messages
- assumes no transmission error when reading data.
- Design IL (internet link) to transmit message over IP.
    - doesn't need flow control.

User Authentication

- each user has an authentication key.
- bilaterial authentication.
- authentication server maintains the db of keys for users.
    - exchange challenges
    - get permission granting ticket from auth server, and secret key
    - similar to kerberos, but avoids reliance on clocks.
    - allow impersonation
- not suitable for text-based service like telnet or ftp.
- log into authenticator with a 4-digit pin


Special users

- no super user
- impersonation mechanism to provide access to admin-like
privileges (kill misbehaving process)
- `none` user. - no password, and very restricted permissions.
    - can only read world-readable files.
    - analagous to anonymous FTP user




