---
layout: page
---

### Abstract/Intro

- attackers gaining control of large numbers of internet hosts poses a threat
to the security of the internet
- analyze magnitude of threat

- worms aren't new, but with advent and spread of internet, the threat
  is imminent.
- develop threat of new techniques for virulent worms
    - hit list scanning
    - permutation scanning
    - internet-scale hit-lists
- surreptitious worms
    - slow spread, hard to detect


## Code Red

- First version compromised MSFT IIS servers with CVE-2001-0500
- bug in v1 caused it to only spread linearly
- v2 fixed the issue and spread faster
    -  targeted whitehouse.gov
- RCS (random constant spread)
- N total number of servers that an be compromised on the internet
- K is compromise rate
- T is time which fixes when the incident occurs
- a is proportion of vulnerable machines compromised
- t is time (in hours)
- Nda = (Na)K(1-a)dt
    - how many more machines (Nda) will be compromised in the next
      amount of time (dt) ?
- machines first infect at an exponential rate, then later at a rate
  equivalent to how fast infection spreads


## Code Red II

- used same vulnerability to exploit -- different codebase
- used a localized scanning technique to pick which machines to infect
    - quickly infected the local network
- nimda: multi-vectors
    - use IIS CVE
    - bulk email
    - copy to network shares
    - add exploit code to webpages
    - scanning for backdoors left by CR2

## better worms

- hit-list scanning
    - collect a list of potential targets first
    - divide list in half and send to newly infected host
    - stealth scans - need to not attract too much attention
    - distributed scanning
        - attacker can scan most of the internet with a few thousand
          machines
    - DNS searches - assemble a list of domains, and use DNS search to
      find IPs
    - spiders: use crawling techniques to find new IPs
    - public surveys:
- permutation scanning
    - all worms share common random permutation of IP address space
    - scan from random index of ip space, iterate until
- topology aware worms
    - use information on infected machine to guide attack vectors
- internet-scale hit list
- updates/control
    - typically have a c2 domain/ip
    - spread information from one infected host to another instead of single C2
    - programmatic software updates
- envisioning Cyber CDC
    - develop robust comm mechanism for gather field information in a
      decentralized way
    - sponsor research in automated mechanisms for detecting worms based
      on traffic patterns
    - develop state of the art program analysis tools
    - establish mechanisms with which to propagate signature describing
      how worms and their traffic can be detected
    - establish mechanisms to propagate signatures
    - track use of different application in the internet to detect
      previously unknown applications to appear
    - analyze threats of
