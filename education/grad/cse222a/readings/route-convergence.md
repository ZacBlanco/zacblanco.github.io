
### Introduction

- internet connected by thousands of ASes
- routing within AS is intradomain protocols
- between AS is controlled by BGP
- BGP policies can cause routing issues
    - route registry not scalable
- paper proposes a set of guidelines for an AS to follow in setting its routing policies without requiring coordination with other ASes


### Interdomain Routing

- ASes connect at IXPs
- physical connection doesn't imply traffic flow
- BGP speaker must decide whether it should include an advertised route
  and whether that route should be propagated
- An AS can control how traffic enters/exit the network by using a
  **multiple exit discriminator** value on its advertisements to other
  BGP nodes
- processing advertisement has 3 steps
    - import policies
    - path selection
    - export policies
- vendors and industry in general have settled on how to express routes
  - AS maintainers just need to follow guidelines
- policies can result in a "route oscillation" where routes never settle

### BGP Model

- BGP system is a clustered graph
    - $$G = (N, V, A)$$
        - N is ASes
        - V is set of BGP speakers
        - E is eBGP peering sessions
    - each BGP speaker belongs to only one AS.

- Hierarchy
    - customers, providers, and peers
        - customer-provider
            - provider carries customer AS traffic
        - peer-peer
            - traffic shared between two (or more?) peer AS
- BGP export policies
    - export to provider
        - AS export routes and routes of customers, but cannot export
          routes learned from other providers/peers (no transit service)
    - export to customer
        - AS export routes and routes learned from providers and peers (transit service)
    - export to peer
        - AS exports routes and routes of customers, but not routes learned from other providers or peers (no transit service)
- Guidelines
    - Unconstrained P2P Agreements
        - AS should always prefer a route via a customer over a route
          via a provider or peer
    - Constrained P2P Relationships
        - same as above, but peer routes can have the same preference as customer routes.
- Guidelines w/ backup links
    - if the route does not container a backup link, use guidelines A/B
    - if the route contains a backup link, the local preference is the
      backup link preference.
    