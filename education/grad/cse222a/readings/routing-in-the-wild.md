

## Intro

- published 1994/5?
- analyze routing behavior or path conditions, stability, routing symmetry.
- AS synonymous with administrative domain
    - BGP scaling lies in the stability of inter-AS routing
        - if routes vary frequently, BGP routers will need to update a lot (bad)

## methodology

- number of internet sites to run a network probing daemon.
    - measure routes with `traceroute`
- two measurement types
    - d1:  measured all virtual paths between two sites with 1-2 day intervals
    - d2: measured between two sites at 60% with 2 hr mean measurement,
      40% with mean of 2.75 days
- also paired measurements (measure `A-->B` and `B-->A`)
- time between measurements were IID
    - proportion of measurements in a given state equal to the amount of
      time the internet spends in that particular state
- only 37 hosts participated
    - miniscule compared to 6.6M hosts and 50k+ NSFNet stub networks
- routes are representative because they include a non-negligible fraction of ASes
    - internet has only about 1K ASes
- samples half of "most important" ASes?

## routing pathologies

- 5-8% of traceroutes failed due to NPD.control failing to contact one of the sites
- found around 50 persistent routing loops!
    - temporary loop is loop resolved in traceroute
        - D1 has two temporary loops, and D2 had 23 temporary loops
- erroneous routing
    - one traceroute hopped to israel instead of going to london on a transatlantic journey
- fluttering (rapidly oscillating routing)
    - occurred most frequently between wustl and umann
        - 17 vs 29 hops - this splitting is technically allowed but
          probably not appropriate. Hard to get good routing metrics
- infrastructure failures:
    - traceroutes failed a number of times
        - d1: 99.7-99.9 success rate
        - d2: 99.4-99.6 success rate
- unreachable hops
    - internet is > 30 hops, need higher TTLs
    - distance not always correlated to hops
        - 3/5 hops for 1500km/2000km distances
        - 11 hops between MIT/harvard both directions
- outages followed typical working hour phenomenon
    - least in 01:00-02:00
    - most in 15:00-16:00


## routing stability

- In general, Internet paths are strongly dominated by a single route, but,as
  with many aspects of Internet behavior, we also find significant site-to-site
  variation
- route changes exist within 10 minute intervals
- 25 instances across 1302 10-min observations

## routing symmetry

- 49% of measurements observed an asymmetric path that visited at least one
  different city
- consider AS instead of city - still 30% of traffic followed asymmetric paths
