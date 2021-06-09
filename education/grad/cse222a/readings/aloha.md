---
layout: page
---


- present, conbentional methods are with wired communications
    - leased lines, phone/dial-up lines.
    - downsides: dial-up lines may be too long, data rates fixed/limited.,
      higher cost
- argue that dial-up lines are wasted most of the time for computer
  communication because they are used in short bursts.
- paper concerned with method for multiplexing a large number of low data-rate consoles through a single radio communication channel
- two 100KHz channels, one for Tx, one for Rx
- no issues with transmission since transmitter can be controlled (only one tx
  on the particular tx channel)
- data transmitted in fixed packets of 80 bytes with 32 bytes of identification and control bits, along with 32 parity bits
    - 29ms at 24,000 baud
- retransmission at random interval if packet is not received
- "peak" users of 324 - but only if entirely simultaneously trying to send a
  message
- channel utilization of only .186 when the traffic over the bandwidth is 0.5
    - unstable after this with unbounded retransmissions
