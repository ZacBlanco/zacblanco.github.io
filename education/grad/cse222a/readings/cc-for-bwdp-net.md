
## Congestion Control for High Bandwidth-Delay Product Networks

## Introduction

- math shows that regardless of queueing scheme --> higher bw-delay product
causes TCP to oscillate between bandwidth
- all previous attempts are eventually oscillatory at some point
- additive policy slow to adapt
- addition of bandwidth or flows slow to adapt
- generalize the ECN concepts
    - split utilization control from fairness control
    - control state within packets


## Design

- think about congestion control w/o backcompat concerns...
- packet loss is not a good congestion signal
- aggressiveness of changes should be adjust based on a feedback loop
- robustness of congestion should be independent of unknown parameters
  (e.g. number of flows)

## Protocol

- join design of end-systems and routers
- senders maintain congestion window and rtt
    - communicate to routers via congestion header
- congestion header has current sender values, and one feedback slot for
  pos/neg number
- XCP sender
    - sender initializes the feedback to desired window increase
    - whenever ack arrives, positive feedback increases sender congestion
      window, negative reduces it
- XCP receiver
    - router must compute feedback to cause system to converge to optimal
      efficiency and min-max fairness
    - router has _efficiency controller_, _fairness controller_
    - make control decision every RTT
        - per-link estimation control timer to calculate estimate
- efficiency controller
    - maximize link utilization while minimizing drop rate and persistent queues
    - looks at aggregate traffic, does not care about fairness
- fairness controller
    - apportion feedback to individual packets to achieve fairness
    - similar principle as TCP, AIMD
- MIMD in efficiency controller with feedback ~1 RTT to maximize bandwidth
  utilization
- estimation errors are ok because estimations change frequently (1 RTT)

## Performance

- Across different bottleneck link speeds...
    - XCP nearly always uses all bandwidth
    - XCP bottleneck router queues increase linearly
    - virtually no packet drops since linear increase in queue size
- across varying RTT propagation delays
    - high utilization
    - low queue size (not necessarily linearly increasing)
    - no packet drops
- across varying number of flows
    - bottleneck utilization decreases and at least matches for large numbers of flows
    - queue length linearly increases
    - no drops

- security
    - all nodes need to comply with feedback in order to work properly...
    - policing agents?
    - 
