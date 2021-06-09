---
layout: page
---

## Wireless Networks

- number of different wireless specs
- wireless media --> multiple access
- overlapping signals can be a problem
    - transmission distance
    - transmission rates
- bluetooth:
    - 10 meters, 2mbps
- WiFi
    - 100 meters, 100-450mbps
- 4G
    - kilometers
    - 1-5 mbps

- wireless medium has challenges
    - sharing efficiently
    - signal attenuation
- allocations determined by FCC
- shared spectrum - spread spectrum over multiple frequencies
- frequency hopping for security
- direct sequence for fault tolerance using data, random sequence of pseudo
  random bits, and the XOR of the streams.
- wireless networks typically have base station connected to wired network
- usually there is a "fixed' portion of wireless network
    - mesh/ad-hoc networking w/o

## 802.11

- defines physical layers operating in frequency bands at different data rates
- original standard with frquency hopping on 1MHz wide bands
- 2nd  using direct-sequence with 11-bit chipping
- 802.11b for 11mbps
- 802.11a for 54mbps (5GHz)
- 802.11g for 54 mbps (2.4GHz)
- also support for lower data rates (6, 9, 12, 18, etc...)
- systems try to pick optimal rate based on environment noise
- wireless collision is a difficult problem
    - hidden node problem - A and C signals collide at node B, but never reach
      one another
- CSMA/CA (collision avoidance)
- RTS-CTS (ready to send, clear to send)
    - on collisions, wait a random amount of time before retrying (exponential
      distribution)
- packet data transmitted after successful RTS-CTS exchange
- most 802.11 deployments use a base stations
    - split routing and wireless signal
- allow devices to roam between APs
    - each device associated with an AP
- selecting AP is  _scanning_
    - send probe frame
    - all APs respond with probe Response
    - node selections AP with association request
    - AP replies with association response
- 4 address in 802.11 frame to identify original sender receiver and possible
  forwarding sender/receiver


## Bluetooth (802.15.1)

- "lightweight"
- low transmission power for small range
- replace peripheral wires
- 2.45GHz band
- single master with up to 7 slaves
- ZigBee - 802.15.4, for extremely low power
-
