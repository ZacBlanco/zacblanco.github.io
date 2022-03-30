---
layout: page
---

- [Berkeley View on Serverless Computing (page 3-8)](https://www2.eecs.berkeley.edu/Pubs/TechRpts/2019/EECS-2019-3.pdf)
- [Serverless in the Wild: Charactizing and Optimizing the Serverless Workloads at Large Cloud Providers](https://www.usenix.org/system/files/atc20-shahrad.pdf)

## Questions

- Today's serverless functions are stateless. How do you think different functions can share data and communicate?
  - functions can communicate a number of different ways. One of the most common
    patterns is by using shared state or memory. Actual implementations can
    potentially use data storage to "communicate" via reads and writes to S3.
    For higher performance, memory-based key-value stores can be an efficient
    means to communication using services like memcached or redis.
- Can you think of any security threats of serverless computing? Bonus points if you can outline a real threat/attack.
  - serverless computing may have different thread models depending on how the
    cloud providers implement the isolation serverless runtime. For example, AWS
    uses its firecracker VMM to implement and boot its serverless functions,
    while other implementations using Apache OpenWhisk (IBM) use containers. The
    threat surface for AWS will be the VM and its access to the VMM. Whereas in
    OpenWhisk, the threat surface is the shared kernel that is used by the
    containers of the serverless functions. On a container-based serverless
    implementation, if an attacker can cause a OOB memory access due to a bug in
    the kernel, then they can potentially take control of the kernel. Doing so
    would give them access to the memory of the other tenant's serverless
    functions.
- Can you think of any other ways to reduce or avoid cold start for serverless computing (other than what the ATC'20 paper talks about)
  - a change in cold starts is always going to impact (1) cost or (2) latency
    (due to reduced cold starts). One way to reduce cold starts is to ask users
    during function creation what the desired 99th percentile latency should be.
    Depending on this value service providers can change the amount charged for
    the application to accomodate the desired latency. If a user has a very low
    latency requirement the provider can charge more for improved performance
    because they will likely need to keep more warm containers available for
    extended periods of time. If the desired latency is higher the cloud
    provider can charge less and change the policy on how often it removes warm
    containers for that particular application


## Summary

### Cloud Programming Simplified: A Berkeley View on Serverless Computing

In this paper, the authors give a historical perspective on the rise of the
datacenter and commodity cloud computing. They further go on the explain what
serverless computing is and how it is the next evolution in the datacenter. They
show the drawbacks of traditional cloud computing and how serverless computing
overcomes the management overhead and difficulties incurred using tradition
rented servers.


### Serverless in the Wild: Characterizing and Optimizing the Serverless Workload at a Large Cloud Provider

This paper looks at characterizing traditional serverless workloads from traces
at a large cloud provider. Using the traces the authors devise a method to
improve performance by looking at the observed invocation patterns of the
serverless functions to find the best time to prewarm containers. This results
in decreased latency to users and optimized resource allocation for cloud
providers.
