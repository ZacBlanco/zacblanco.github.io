## Introduction

- slow internet problems is the result of congestion control
- loss-based congestion control keeps buffers full and causes memory bloat.
- slowest link in a route determines data delivery rate
- queue shrinks only when departure rate exceed arrival rate
- TCP views arbitrarily complex routes as a single link


- Characterizing bottlenecks
  - TCP runs at peak performance  when bottleneck packet arrival is the
    bottleneck bandwidth and the total data in flight is equal to the BW-D
    product
  - bottleneck bandwidth and round trip propagation are constantly changing - so
    they must constantly be estimated
  - `RTT = RT_prop + n_t`
    - `n_t` is noise introduced by queues along path
  - estimate of bottleneck bandwidth is the delivery rate of packets.
  - bottleneck bandwidth calculated by max delivery rate in a sliding window
    (6-10 RTTs)
  - impossible to know both bottleneck bandwidth and RTprop at the same time

- sometimes acks are grouped together, causing delays in updates to bottleneck
  bandwidth
  - set cwnd_gain to a alrger value (2) to account for additional RTT time.
- 
