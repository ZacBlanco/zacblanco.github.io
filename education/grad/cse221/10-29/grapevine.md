---
layout: page
title: CSE 221 - Operating Systems - Notes on "Experience with Grapevine; The Growth of a Distributed OS"
permalink: /education/grad/cse221/10-29/grapevine/
keywords: CSE, 221, UCSD, UC, San Diego, software, operating, system, linux, C, rust, OS, distributed, grapevine
description: Notes for "Experience with Grapevine; The Growth of a Distributed OS"
mathjax: true
---



> Q: In what ways was Grapevine explicitly designed to handle scalability?

Answer:

They distributed messages closest to where users were located because the
server-server connections were quite slow, so they made conscious decisions to
move message storage close to where the messages would be accessed.

They also made decisions which changed how they handled larger amounts of
registered users. Needed to put registries close to users.


## Introduction

- Published 1984
- Distributed, replicated system to provide message delivery, naming,
  authentication, resource location, access control services to an
  internet of computers


## Structural Overview

- Written in Mesa w/ Alto computers
    - 128KiB mem
    - 5MiB storage
- Built on internet protocols
- message and registration service (two programs)
    - all servers cooperate
- message server holds messages for clients
- message server may accept request from anyone at any location
- registration handles naming, authentication, ACLs, resource mapping.
    - based on a registration database
    - maps names to user information, distribution lists, etc
- Two types of entries in registration DB
    - group and individuals
- Entry in DB is an RName.
    - Group entry is a set of RNames
    - individual entry is a password, list of inbox sites, connect site, etc
- Two-level naming scheme for an RName (`<F>.<R>`)
    - R is registry name
    - F is a name within registry
- Registration DB is distributed and replicated
    - multiple copies, no SPOF
- registrars (humans) responsible for the registry entries
- GrapevineUser package as client
- System handled upwards of 8500 messages/day

## Effects of Scale

- Initial target of 30 grapevine servers with 10k users
- manifestation of system size is total space required to store
  configuration information
- configuration information must be stored on all servers
- distribution lists not scaled well
    - some lists always increased as a linear fraction of total user community
    - grapevine server needed to determine best inbox site for each recipient
    of a list. This proved too time consuming for large lists
    - fix by associating sublists with registries rather than individuals
- doesn't use a reliable protocol - too many hops might not guarantee delivery
- manual placement of replicas, registries, and inboxes
- add new servers when load on one server is too great
- assigned servers to geographical locations rather than local communities
    - e.g. palo alto instead of "building 35"
    - would have been better if they used organization instead
- Five considerations for registry location
    - make register data easily accessible to message servers
    - contents on distribution list close to message servers accepting
      such messages
    - Registry data much be available to all clients even when network
      links break
    - have enough replicas to make probability of losing registry data due to
    hardware failure essentially 0.
    - avoid overloading any given registration server


## Transparency of Distribution

- eventual consistency of entries can be an issue
- sometimes duplicate messages appear
    - recipient list of a message may have names listed twice.
- deliveries may be placed in a non-local server due to scheduling
    - cause delayed deliveries compared to waiting for a local server
- originally designed to become a source of authentication and access control
    - previously, each file server maintained its set of users
    - cache authentication request results in file server instead
- even with caching, authentication requests could be slow due to group nesting
    - nested groups may not be in same registry
    - 5+ minutes to discover a group or user doesn't exist
- added new mechanism to arbitrarily read and store mail.
    - grapevine intended as a buffer, now became semi-permanent mail storage
    - did not re-design for this use via PCs


## Operation of Dispersed System

- Monitoring, control, and repair must be accessible through internet
    - geographically distributed
    - all repair is simple by loading programs/reboots - minimal training
- determined message bodies are not replicated - lost bodies are replaced with
  an apology.
- "viticulruist's entrance" for remote monitoring - kind of like ssh
- logging of actions

## Reliability

- must make the service always available
- software should be bug free
- reject connections if resources too utilized (e.g. disk space < 5% remaining)
- failure of one machine could lead to domino effect of other servers
  failing due to overload
- reluctant to fix some issues because of weariness of introducing new
  bugs that affect the user community.


## Lecture Notes

- classic distributed system
- remembered for design decisions, tradeoffs, and success
- wanted to understand building, deploying, and maintaining a large system
- Goals
    - distributed message and registration service
    - provide mail, naming, and access control
    - tradeoff between transparency and ease of design/implementation
- Two services
    - messaging
        - delivery of messages
        - buffering of messages
            - messages only buffered until downloaded
        - any message server accepts any message for delivery
    - registration
        - naming, records of people
        - tables aps names of users to machines, services,etc.
        - registries correspond to locations, organizations, applications
        - authentication
            - user passwords
            - like YP/NIS/LDAP in Unix, Domains in NT
        - Access Control
            - ACLs for files in distributed system
- how did they depend on one another?
     - msgs: names locations, groups from reg service.
     - reg: use message service to maintain distributed registry.
- how were clients connected to server?
    - connected via local ethernet
- servers connected via low-bandwidth modem lines
    - frequent network outages
- scaling
    - cost of computation should not grow as size of system grows
        - easy to understand, difficult to implement
    - usually some kind of ceiling
    - can you afford complete info in some areas?
        - e.g. registration servers know the names, address of all message
        and registration servers and servers with replicas
        - location algorithm chooses nearest server from all
    - grapevine scales by adding servers of fixed power (instead of increasing
      single server power).
      - manifestation of scale
        - membership of list grows
            - time to deliver list grows
            - frequency of add/delete grows
                - send only the deltas
            - updates increase freq with growth in user population
- transparency
    - achieved via distribution and replication
    - poor aspects of transparency
        - updates are not transactions
            - takes time to propagate, users see inconsistent state
    - duplicate message delivery
        - name appears on multiple lists
        - server for a list is down, message only sent to live servers
        - when server comes back up, message is sent again.
- replication
    - every user has to inboxes
    - registry is a unit of distribution
    - no server contains all registries
    - each registry is replicated.

- complete transparency is impossible, not always a good decision
    - user wants to know about _what_ failed exactly
    - key decisions about deciding what to make transparent, and what isn't
- system designers should consider admins and users
