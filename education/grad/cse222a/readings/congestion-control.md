## Congestion Control

- how to fairly allocate resources to competing users?
    - need to share bandwidth, of links, buffers of routers
    - overflowing queues is **congestion**
- congestion control and resource allocation are two sides of the same thing
- "queueing disciplines" control ordering of transmission and which packets are
  dropped


### Issues in Resource Allocation

- _resource allocation_: process by which network elements try to meet competing
  demands for resources
    - usually link bandwidth
- _congestion control_: efforts made by nodes to prevent/respond to overload
  conditions
- `flow control != congestion control`:
    - flow control is keeping a fast sender from overrunning a slow receiver
    - congestion control is managing a set of senders sending too much data
- assume connection-less flows
- routers maintain some state for each flow
- flow not explicitly created


### Taxonomy

- resource allocation mechanisms:
    - router vs host centric
    - reservation vs feedback based
        - ask in advance vs change behavior based on performance
    - window vs rate based
        - window: amount of data that can be received
        - rate: send purely based on expected consumption rate

### Evaluation Criteria

- effective resource allocation
    - consider throughput and delay (maximize/minimize)
    - allocation effectiveness = power = throughput / delay
    - need to find the "sweet spot", too litte and no enough throughput, too
      much throughput and delay increases
- fair allocation != equal allocation


## Queueing Disciplines

- FIFO
    - fifo is queueing practice, tail drop on full queue is drop policy
    - simple modification is priority queueing
        - read ip header for service type, queue for each service
            - priority of services: poll high priority queues first
- Fair Queueing
    - multiple flows with queues, round-robin dequeue from each flow
    - simple segregation..not involved in flow setup
    - need to take into account packet length
    - approximate bit-level fairness, but cannot preempt packets


###  TCP Congestion Control

- tcp source needs to determine capacity of network
- use ACK arrivals as signal that network is capable of handling more traffic

- Additive Increase/Multiplicative Decrease
    - each connection has state called the `CongestionWindow`: limits amount of
      data in transit
    - TCP effective window: `MaxWindow = min(CongestionWindow,
      AdvertisedWindow)`
    - `EffectiveWindow = MaxWindow - (LastByteSent - LastByteAcked)`
    - amount to change the `CongestionWindow` called AIMD
        - change based on packet observations
        - add ~1 if success, halve if packet dropped
- Slow Start
    - with AIMD, can take time to ramp up connection speed
    - optimization to rapidly increase sending rate up to desired value
        - additive increase once reaching the right level
    - store temp variable as "target" congestion window to rapidly increase up
      to the value
    - could try to ask for rate in SYN packets, upon connection est. will have
      value applicable to all routers on path
        - requires router cooperation/support
- Fast Retransmit+Recovery
    - lost packets and waiting for timeouts....super slow
    - send duplicate acks to sender. Duplicate ACK == out of order send ==> some
      packet dropped
    - once seen a number of duplicate acks -- retransmit
    - use acks to determine congestion windows, removing slow start problem
- TCP CUBIC
    - default algorithm implemented in linux
    - adjust congestion window at time intervals instead of only when acks
      arrive
    - uses a cubic function to adust the congestion window
    - 
