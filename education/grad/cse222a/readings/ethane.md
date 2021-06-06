## Introduction

-  new network architecture for the enterprise
- enterprise networks comprised of applications, protocols, w/ reliability and security constraints
- management solutions are weak which makes network management expensive and error-prone
- approaches
    - physical middle-box to route all traffic through
    - change and/or layer more protocols
- ethane -- three principles
    - network should be governed by policies declared over high-level names
    - policy should determine the path that packets follow
    - the network should enforce a strong binding between a packet and its origin
- additional ethan features:
    - security follows mgmt
    - incrementally deployable


## Ethane design overview

- ethane control the network by not allowing any communication between
  end hosts without explicit permission.
- central Controller with global network policy
- 2nd component is a switch: simple/dumb with flow table and
  communication channel to the controller
- names/binding/policy language
    - controller needs host/service-type IP mappings
    - controller knows about types of machines _and_ where they are located (physical port, AP, etc)
- five basic ethane activities
    - registration (with controller)
    - bootstrapping (connect to switch to controller)
    - authentication
    - verify users are allowed to have packets routed in the network
    -  flow setup: when a packet is un-recognized, switch forwards it to the controller for decision
        - following packets use same decision
    - forwarding: if a controller allows a path, sends the packet back
      to a switch to forward it based on a new flow entry
- ethane switches only have a partial view of the network, controller has entire view
- switches in the network can be much "dumber" and replace een routers
    - controller takes care of telling the switch where packets should be routed
    - ethane switches need to maintain much less state than a forwarding switch
- flow table and flow entries
    - switch datapath is a managed flow table
    - flow entries have a header to match packets against and corresponding rule
    - per-flow entries and per-host entries
    - only controller can add entries into a flow table
    - flow table implemented with two exact-match tables -- can use
      exact matches+hashing than TCAMs (longest-prefix)
    - switch needs a small local manager/OS to establish and maintain a channel to the controller
    - if switch is not in the sam broadcast domain as the controller, use IP tunneling

- controller
    - registration: all entities needing to be named by the network
      (hosts, protocols, switchers, users, APs) must register
    - authentication: all entities must authenticate with the network
    - track bindings: tracks connections between names addresses, and physical ports
    - namespace interface: a way for a management entity to view/analyze network behavior
    - authorization: controller must determine which entities have access to particular resources
    - enforce resource limits: must be able to manage resource
      utilization of entities in the network
- broadcast and multicast
    - switch holds bitmap to determine flow destinations
    - controller builds multicast tree
- replicating the controller
    - use journaling+replication (e.g. paxos/raft) to keep consistent state in case of failure
- link failures:
    - switches send neighbor discovery messages to neighbors to determine link state
    - controller can receive said messages as a "health check"
- bootstrapping
    - use MST algorithm to get packets to the controller.
- pol-eth policy language
    - network policy declared as a set of rules
        - each with corresponding condition and action (allow/deny)
        - rules must be specifically allowed
    - each rule independent

### results

- flow setup time increases with controller load...about 1.6ms for 10k+ node network
- shortcomings
    - broadcast traffic takes 90% of flows
    - trusts end hosts to not relay traffic around policies
    - assumes transport port numbers corresponds with traffic type (e.g. 80/443 for http, 22 for ssh)
    - ethane switches rely on ethernet addresses to identify flows
        - spoofing a MAC can fool ethane into delivering packets to the wrong hosts
    -  