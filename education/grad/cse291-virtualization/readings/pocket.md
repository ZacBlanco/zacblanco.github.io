---
layout: page
---

- [Pocket: Elastic Ephemeral Storage for Serverless Analytics (OSDI '18)](https://www.usenix.org/system/files/osdi18-klimovic.pdf)

## Questions


- Why isn't using existing in-memory key-value stores such as Redis and
  memcached a good option for storing ephemeral data in serverless computing?
    - None of the current services are optimized for fast access to ephemeral
      data at a reasonable price point. Either the performance is too poor or
      the cost is too high. Ephemeral stores need to delivery high bandwidth,
      low latency, and high IOPS for all object sizes. Unfortunately, redis and
      memcached are not optimized for all of the these workloads. They can
      provide low latency for some objects and high IOPS for some objects, but
      not others.
- How does Pocket balance storage load?
  - Pocket balances load by steering data to specific nodes in order to manage
    traffic. pocket controls the destination of data by assigning specific
    weights for each storage server in a job's weight map. To balance the load,
    the Pocket controller will give a higher wight value to under-utilized
    storage servers. This helps reduce latency and increase IOPS for the entire
    cluster
- Do you think Pocket solve all the problems of managing states in serverless
  computing? If not, what do you think are the remaining problems?
  - No, I don't believe pocket solves all of the problems because it requires
    using a new interface for specifically for storing and retrieving data that
    it would have to convince other applications to utilize otherwise. Ideally,
    the storage interface would require less change to the applications or use
    traditional POSIX interfaces for communication. Additionally, Pocket does
    not touch much on the security aspects of managing state. There are
    certainly considerations that need to be made if what was once stored only
    in applications' virtual memory is now being sent and stored over the
    network servers where potential snooping could be involved.


## Summary

This paper presents Pocket which is a data store that is designed to be utilized
by serverless functions in order to share ephemeral data between one another at
the required performance and cost levels to make it attainable for serverless
functions to reliably share and communicate state. It does this by utilizing a
range of techniques such as smart balancing of data across the storage cluster
and by using hints to notify the system of particular events which allow the
cluster to optimization its resource allocations.
