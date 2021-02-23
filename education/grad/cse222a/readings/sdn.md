### Software Defined Networking

- Approach to building networks favoring programmability
- requires strongly defined set of APIs

### Software Stack

- bare-metal switch running a  _switch OS_ 
    - controlled by global "Network OS"
        - netOS hosts _control applications_
- many open source components for each piece of the stack..
- switch vs host impl
    - network OS spans many switches
    - each switch has its own OS
- SDN software stack runs on end hosts
    - virtual switch responsible for forwarding packets to and from VMs
    - similar to physical switch, vswitch forwards packets from input to
      output ports
- smartNIC offloads computation to NIC (TCP checksum)

### Bare-Metal Switch

- network data plane implemented by a set of connected switches
- architecture agnostic to switch vendor
    - programmable switches have P4 programs defining the forwarding
      pipeline
- basic switch behavior defined as a P4 program


### Switch OS

- each switch runs a local switch OS
    - runs on commodity processor
    - responsible for API calls issued to the switch
    - many switch OSes
        - SONiC (Azure)
        - Stratum (Google)
            - mediates all interactions between switch and outside world
        - Open Network Linux (ONL)

### Network OS

- Network OS responsible for configuring and controlling a network of switches
- Runs on centralized SDN controller
- maintains global view of topology
- control apps instruct network OS to control packet flows to underlying
  switches according to prescribed service
- critical subsystem is scalable KV store
- atomix implements KV store (fault-tolerant, with RAFT)

### Leaf-Spine Fabric

- control app (e.g. Trellis) dictates network topology
- leaf switches which are ToR switches, and spine switches connected ToRs
- trellis is
    - switching fabric for servers
    - connects cluster as a whole to downstream networks
    - interconnect at network edge
- specific control apps for network features
    - VLAN/L2 bridges
    - ipv4/6 unicast/multicast
    - DHCP L3 relay
    - Dual-homing servers and routers
    - QinQ forwarding/termination
    - MPLS-based pseudo-wires
     