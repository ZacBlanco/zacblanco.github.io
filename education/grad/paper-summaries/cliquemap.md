---
title: CliqueMap; Productionizing an RMA-Based Distributed Caching System
type: page
conf: sigcomm
year: 2021
link: https://dl.acm.org/doi/pdf/10.1145/3452296.3472934
---


## intro

- focuses on design, impl, experience
  - productionizing an remote memory-based RPC framework
- empty RPC overhead is 50us
- reduce overhead using RMA
- challenges:
  - designing performant data structures and protocols accomodating concurrency between RMA and RPCs
  - meeting productionization expectations (availability, planned maintenance, mem mgmt, interoperability)

goals:

1. provide performant lookups
2. careful division of responsibilities between RPC and RMAs
3. support or productionization expectations


## Productionization ideas

- hybrid RMA/RPC system
  - RMAs for common-case GETs, but RPCs for complex mutations and other traffic

- self-validating responses
  - KV pair guaraded by checksum across key, value, and metadata
  - detect "torn read"
- backend data structures
  - 2 RMAs, 1 get for bucket region, 1 get for data entry

- on SETs, the code is "easy" bc it's implemented via RPC
  - 



