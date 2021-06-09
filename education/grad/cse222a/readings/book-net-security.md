---
layout: page
---

## Trust and Threats

- security eventually fails -- assumption on **trust** of another
- threat is just a _potential failure scenario_
- security comes with a cost (usually)
- research into a _trusted computing base_
- original internet did not consider threats from _within_ the network

## Example Security Systems

- varied layers of security implementation depending on applications
    - Application to IP
    - PGP, SSH, TLS/SSL, IPSec, 802.11i

## PGP

- provides authentication, confidentiality, data integrity
- relies on a "web of trust"
- sender/receive with public/private key
    - sign message with private key
    - encrypt message with newly generated one-time session key
    - encrypt session key with bob's public key, and append that to the message
    - base64 encode the ascii-compatible representation
- PGP doesn't defend against lost mail, but alice can at least be guaranteed only bob can read the message


### SSH

- 3 protocols
    - SSH-TRANS
        - establish secure communication channel - similar to TLS?
        - make assumption typically when connecting that the server you connect to is "legit"
        - server always starts connection with its public key
    - SSH-AUTH
        - user authenticates with password/cert
    - SSH-CONN


### TLS

- SSL originally developed by netscape
- negotiate cryptography algorithms at runtime
    - data integrity hash
    - secret-key cipher for confidentiality
    - session key establishment
    - (optional) compression
- session-based handshake to obtain "master secret"
    - client-server: list of algo combinations
    - server-client: chosen algo combinations
    - server-client: server authentication msgs
    - client-server: client authentication messages
    - client-server: HMAC of master secret and previous messages as seen by client
    - server-client: HMAC of master secret and previous messages as seen by server
- record protocol
    - within a session, TLS adds confidentiality to underlying transport. data sent through tls is:
        - fragmented/coalesced
        - optionally compressed
        - integrity protected with hmac
        - encrypted with secret-key cipher
        - passed to transport layer for transmission
    - TLS can resume a session (optimize away handshake)
        - originally due to bad original http implementation
- TLS1.3 improves resumption to send a secure ticket instead of unsecured session ID

## IPSec

- security implemented at lowest layer
- required in ipv6
- IPSec provides
    - Authentication Header (AH)
    - Encapsulating Security Payload (ESP)
    - Internet Security Association and Key Management Protocol (ISAKMP)
- Security association (SA) is a one-way connection with security properties
- ipsec uses an SPI (ID number on IP packet)
    - SA established/negiotated, modified, deleted with ISAKMP
- transport mode and tunnel mode
    - transport mode acts like normal TCP/UDP
    - tunnel mode encapsulates data as an IP packet
- ESP has header an trailer on data in tunnel mode

### 802.11i

- standard for encrypting wireless traffic
- WEP broken early
- WPA3 replacement
- WPA result is a pairwise master key
- personal mode uses weaker security (PSK)
- EAP (extensible authentication protocol) used between AP and Authentication server to check if device can connect to network
- need to have mutual authentication between AP and AS
- after obtaining pairwise master key, AP and device execute session establishment protocol with a 4-way handshake
    - result is Pairwise transient key (set of keys), session key, temporal key

### Firewalls

- blocks network traffic on particular network ports
- assumes internal computer is trusted zone
- NAT+firewall occasionally found in same device


