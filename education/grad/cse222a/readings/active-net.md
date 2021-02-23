## Introduction

- Active networks allow users to inject customized programs into nodes.
- can permit new computations to happen in-network
- advantages of active networks by exchanging active programs rather
  than passive packets
    - exchanging code has a basis for adaptive protocols
    - capsules provide a means of implementing fine-grained
      application-specific functions
    - programming abstract is a powerful platform for user-driven customization


## Lead users

- firewalls
    - implement filters to determine which packet should be passed transparently
    and which should be blocked
    - tend to implement application and user-specific functions in addition to
      routing
    - active network could considerably automate process of adding new functions
      or rules into firewalls
- web proxies
    - cache and serve www traffic
    - algorithms to automatically balance cache hierarchy by re-positioning
      caches themselves
    - web pages with js code make the case for programs to dynamically generate
      page content in network on the cache side
- mobile gateways
    - observes and adapts connection to network depending on physical conditions
    - w/ active network, can choose to compress traffic (or not)

- other domains
    - multi-point communication strategies
    - information fusion > fuse within the network

## Active Networks

- discrete approach to programmable switches
    - processing is separate from program loading
    - header message identifies the program to be run
    - operators allowed to load code into routers
- capsules - integrated approach
    - every message sent is a program
    - program + data
    - capsule execution can result in 0 or more outgoing capsules
- programming with capsules
    - simple application: compute next hop + schedule outgoing packets
    - active routing - capsule can dynamically enumerate and evaluate paths
      while being routed

- ways to reach beyond capsule environment
    - foundation components -- universally available services outside of capsule
        - capsules may require access to service such as routing tables, link
          states, etc. Need a standard library of services
    - active storage -- modify node storage after capsule execution complete
        - leave information behind for new capsules to read
        - "flows" rather than connections
    - extensibility -- allow programs to define new classes/methods
        - inefficient for programs to be always alongside data
        - need a way to push some of the code into the network device with small
          reference to run program in-packet
- interoperable programming model
    - capsules require mobility
    - must be transmittable across network, portable
    - must also be safe+efficient


## Capsule Programs

- capsule primitives
    - list of primitive actions a capsule may perform
    - all nodes on a path must be capable of executing the program
- safety and efficiency
    - restrict execution primitives available to programs
    - programming language to foster efficiency
    - can go with interpreted (e.g. python), intermediate (java bytecode) or binary (platform dependent)
    - run-time compilation of C programs
- java-like instruction-set

## Node Resources

- need basic resource specification, platform independent
    - transmission bandwidth
    - processing capacity
    - transient storage
- link abstraction to encompass unit of bandwidth allocation and traffic
  patterns
- instruction execution -- assign capsules default allocations (# of instr?)
- transient storage for aggregation of capsules - plus garbage collection
- active storage -- soft+persistent
    - soft: cache objects like hints/flow state. do not survive restarts
    - persistent..self-explanatory

- resource safety
    - safe manipulation of node resources: partitioned into few activities
    - validation of user requests for resources
    - automated  delegation of resource authorizations


## Internet to ActiveNet

- researchers should attempt to deploy wide-area ActiveNet
    - overlay on traditional infrastructure
- 
