---
layout: page
---

- [Kubernetes](https://kubernetes.io/docs/tutorials/kubernetes-basics/)
- [gVisor](hhttps://gvisor.dev/docs/architecture_guide/)



## Questions


- What is a Kubernetes Pod? How do you think it is useful in container
  orchestration?
    - a kubernetes pod is a set of processes or containers within the same zone
      that may interact with or share the same set of system resources allocated
      by the kubernetes control plane. They are always scheduled as a unit. This
      is useful in container orchestration because it allows multiple related
      processes to function and utilize the same resources (e.g. a shared pipe)
      without needing to explicity expose those resoures to additional pods (a
      potential security risk)
- What does Kubernetes use etcd for? Why is having a consistent, atomic
  key-value store important for Kubernetes' control plane?
    - kubernetes needs etcd to maintain a fault-tolerant consistent view of the
    cluster's state. This state includes resource allocations, scheduled pods.
    Without a consistent centralized view of the cluster state, scheduling decisions
    become much more difficult.
- Vulnerabilities in the Linux kernel makes it unsafe for containers to call
  Linux system calls. How does gVisor solve this problem?
    - it "sandboxes" the container by intercepting system calls from the
      container and sending the syscall requests to a separate kernel which
      emulates the syscall. Even if there is a bug in the implementation of the
      separate kernel it prevents the host from being compromised.


### Summary

#### Kubernetes

The Kubernetes documentation covers all of the concepts required to understand
Kubernetes and how its container orchestration functions. This is critical to
maintaining and working with deployments.

#### gVisor

gVisor is a method of running containers with additional isolation to prevent
potentially rogue containers from attacking the system at the host layer. This
results in additional layer of security for potentially untrusted applications.

