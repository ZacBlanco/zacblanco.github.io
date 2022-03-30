---
layout: page
---

- [Firecracker: Lightweight Virtualization for Serverless Applications](https://www.usenix.org/system/files/nsdi20-paper-agache.pdf)

## Questions


- What is the benefit of Firecracker over gVisor in terms of the specific goals
  Amazon has for their cloud production environments?
    - gVisor introduces additional overhead due to the way syscalls are implemented.
    It also doesn't have as strong of a security boundary because it simply uses
    an additional kernel message-passing based implementation to secure against
    kernel attacks, while the same underlying kernel is still used. Firecracker
    uses a full VM which has an even smaller attack surface than gVisor.
    Additionally, with VM startup overheads comparable to docker they also achieve
    the low startup overhead goal required for serverless applications.
- What mechanism(s) allow Firecracker to run thousands of MicroVMs on the same
  machine (with 10x-20x oversubscription rate)?
    - firecracker uses a "soft allocation" model where the resources for a
    particular function are not actually reserved, but rather given a
    statistical probability that their workload will complete without resource
    contention. Using statistical probabilities for resource allocation means
    that AWS can provide generally non-contending performance in most cases
    while still fully utilizing server resources to their maximum ability.
- Why do you think Firecracker (when deployed to power AWS Lambda) run one
  process (one slot) in one MicroVM?
    - They run only a single slot per-vm because otherwise the added security
    benefits of using VMs (rather than containers) are lost. The added security
    of firecracker comes from using VMs rather than containers.


## Summary


AWS Firecracker is a lightweight VMM designed to run container workloads
within a VM rather than a container. Doing so provides much better security
guarantees for the users of the AWS lambda service. AWS overcame the challenges
with other VMMs in firecracker by using the smallest possible amount of drivers
and a minimal kernel to boot each OS image. This meant the VMs had similar
overhead to containers but with better security guarantees, making for a more
secure and stable cloud product.
