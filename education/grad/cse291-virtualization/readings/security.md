---
layout: page
---

- [When Virtual is Harder than Real: Security Challenges in Virtual Machine Based Computing Environments](https://www.usenix.org/legacy/event/hotos05/final_papers/full_papers/garfinkel/garfinkel_old.pdf)
- [Hey, You, Get Off of My Cloud: Exploring Information Leakage in Third-Party Compute Clouds](https://cseweb.ucsd.edu//~savage/papers/CCS09.pdf)

## Questions

- Can you think of some drawback of enforcing security mechanisms at the hypervisor level (compared
  to at the guest OS or above)?
  - one of the nice parts about virtual machines is that because you usually deal with _virtual_
  hardware, you can't severely affect the underlying machine, other than using a few resources.
  Because the VMs have virtual hardware, you can let the OS give you _full_ control over the VM and
  its corresponding OS environment. This leaves you free to modify and test low-level changes such
  as kernel modules, device drivers, or changing/updating the OS-installed packages. Making these
  changes on a VM rather than a bare metal machine makes is less time consuming and easier to fix in
  the case of errors. If more security mechanisms were enforced at the hypervisor level to prevent
  some of these types of use cases, then VMs would lose some of their usefulness.
- When a zone and/or an instance type are used more frequently (i.e., having higher loads from more
  tenants), do you think the co-location attack would be come easier or harder? Why?
  - when a zone or instance is used more frequently, this means the number of available slots
  to schedule a particular machine becomes lower. At high usage points, this might make the ability
  to predict the physical location easier, as the number of available slots would be lower. However,
  it might also be more difficult to get the attacker's machine scheduled to be co-resident with
  a victim machine if there is a smaller number of slots available because the scheduler may be more picky
  about distributing the available resources. However, when there is lower usage of an instance
  type or availability zone, the number of possible ways to co-locate with a victim machine is higher
  which might make it easier to schedule an instance to be co-resident with a victim VM
- Do you think a similar co-location attack exist with serverless computing (i.e., one function
  attacking another function on the same physical machine)? Does serverless computing make such
  attacks harder or easier and why?
    - a similar co-location attack could theoretically be carried out on a serverless platform,
    however, the realistic odds of it working are much lower. This is due to the more dynamic
    scheduling nature of serverless functions. EC2 instances are scheduled and alive on the scale of
    minutes at a time. This leaves more time for an attacker to determine co-residence and launch
    the attack. On the other hand, serverless functions are scheduled on the order of seconds to
    milliseconds, which leaves a much smaller gap for an attacker to determine co-residence and
    launch the attack. However, on serverless computation, multiple instances of a function can be
    executed quite easily, so the chance of an attacker landing on a co-resident machine with an
    application may be higher or lower depending on the service load.

## Summary

### When Virtual is Harder than Real: Security Challenges in Virtual Machine Based Computing Environments

The authors' take on this work shows that they see how VMs were beginning to change the IT and
infrastructure landscape within large organizations. They dig up the security issues that system
administrators already have to deal with, and show that VMs multiply their headaches by 1000x due to
the increased attack surface that multiplying the number of "machines" on the networks creates. They
examine some possible solutions and how they can make the IT department's worst nightmare a little
less scary.


### Hey, You, Get Off of My Cloud: Exploring Information Leakage in Third-Party Compute Clouds

The authors' of this paper demonstrate how information between co-resident VMs on the same physical
server can be leaked to other tenants machines. They also show how it is possible to take advantage
of some of the commodity public cloud schedulers to make them more likely to place VM instances
on the same machine in order to mount these side-channel attacks.
