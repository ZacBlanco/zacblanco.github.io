---
layout: page
---

## Intro

- Choosing proper boundaries between functions is the primary activity
  of system designer
- discusses class of function placement that is used without explicit
  recognition
- system w/ communication has a firm interface between communication
  subsystem and the rest of the system
- function in an application can completely and correctly be implemented
  with only the knowledge and help of application standing at the ends
  of the communication system.
- providing a function as a feature of the communication system is not
  possible (since both ends need it to communicate properly)


## E2E Caretaking

- consider a file transfer program from host A to B
    - threats:
        - reading file from original disk could be corrupted
        - errors in copying or buffering data from A or B
        - CPU or memory transient error during buffering/processing
        - communication system drops or changes bits
        - one or both hosts may crash
- is adding network features to the comm system useful to the application? (redundancy, packet checksum, etc)
    - argument that while it eliminates one of the threats, in the end it is really only reducing the number of retries.
    - communication system adding reliability does not decrease the work
      of the application developer
- bug in reliable system caused headache for users in MIT networks
    - gateway was flipping 1/1M bytes

## Performance Aspects

- reliability of networks is for performance rather than correctness
    - large file transfer with unreliable packets -- small probability
      to succeed with many retries - reliable transfer increases
      successful network transfer but doesn't eliminate other issues
- applications pay, even when they don't want/need it


## Other examples

- Delivery Guarantees
    - ARPANet used RFNM (request for next message) which isn't really
      useful to applications because the apps want to know if the
      receiving end acted on the message, rather than if it is just
      requesting the next
- Secure data transmission
    - authenticity of messages still needs to be checked, even if the data
    coming over the wire is encrypted.
- Duplicate message suppression
    - Applications may duplicate messages anyways during failure/retry scenarios, so why should the network protocol suppress them?
- Guaranteeing FIFO Message Delivery
- transaction mgmt

## Identifying the "Ends"

- need to analyze application requirements (e.g. voip vs file transfer)
