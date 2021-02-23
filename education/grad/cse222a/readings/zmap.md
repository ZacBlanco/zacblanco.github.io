## Abstract/Intro

- internet scanning can expose vulnerabilities and track adoption of defense mechanisms
- probing entire address space is difficult/slow
- zmap designed for internet-wide scanning
    - optimized probing
    - no state per-connection
    - no retransmission
- good for measuring
    - protocol adoption
    - visibility into distributed systems
    - high-speed vulnerability scanning
    - uncovering unadvertised services


## probe methodology

- uses random permutation to scan maintaining negigible state
- "multiplicative group of integers modulo "p""
    - p is 4,294,967,311 (closest prime to 2^32 --> 2^32 + 15)
    - multiplicative group
    - find a root once, then it is constant time to the next address
- sends raw ethernet packets to avoid routing lookup, arpcache lookup,
  and netfilter check on every packet.
- libpcap to handle responses -- easily able to handle because most
  hosts don't respond
- receiver needs to start before the scan starts, and wait until after
  the scan ends (for delayed packet responses)
- probe modules
    - half-closed probing to limit the number of packets transmitted
- put a MAC hash in unused packet fields
    - verify in response of incoming packets


## validation + measurement

- essentially all-stock software/hardware configuration
- scan rate does not affect the number of hosts foudn
- 1 SYN packet will achieve ~98% coverage
- diurnal affects on hosts observed
    - hit rate changes on a diurnal pattern.
- nmap is excruciatingly slow to scan compared to zmap
    - take over a year to scan the entire internet
    - zmap finds more hosts than nmap due to TCP connection timeout settings on
      linux
- did SSL certificate scan far faster than previous studies


## applications and security implications

- can more quickly detect malicious behavior
- also opens up more opportunities to be malicious..
- can take mere hours to scan and infect entire internet...
- detect weak key usage to notify administrators
- discover unadvertised services
    - e.g. Tor nodes
- scan for outages/availability
    - hurricanes?


## good internet citizenship

- 