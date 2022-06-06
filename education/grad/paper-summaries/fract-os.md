---
---

FractOS:

### Intro

- most systems require control to come back to the CPU
    - e.g. GPU can't talk directly to the FPGA


- goal: decentralized execution of application logic over disaggregated
  heterogeneous resources
    - P2P data transfer
- big challenge: security
- devices need to speak FactOS APIs
- devices need to be shared securely
- adaptors implement RPC interfaces and memory/buffer access
- continuation-based RPCs - to know where the next request has to go


### Motivation

- can't rely on CPU to perform all application control flow
- devices need to know _which device do I send to next?_
    - if they implement this, decentralized applications are possible

### Design

- programming abstractions:
    - memory object
    - request object
- controllers deployed on CPUs or smartNICs
    - usermode linux daemons
- processes invoke fractOS operations
    - memory/request

#### Memory/Request Objects

- objects rely in a single global namespace
- capabaility is a reference to an object
- references encapsulate actual network address of the service
- global namespace managed as a KV store where services can publish capabilities
    - publishing enables access
- memory capability: read memory buffers anywhere in the system
- memory can allocated or copied, or _dimished_
- request objects
    - represent service RPC endpoints
    - process implementing the service must create a request representing that
      RPC
    - clients which obtain the request are granted authority to invoke the RPC
      of the provider
    - request may have arbitrary arguments


