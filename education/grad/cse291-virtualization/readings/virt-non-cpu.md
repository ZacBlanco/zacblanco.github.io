---
layout: page
---

- [A Full GPU Virtualization Solution with Mediated Pass-Through](https://www.usenix.org/system/files/conference/atc14/atc14-paper-tian.pdf)
- [Sharing, protection, and Compatibility for Reconfigurable Fabric with AmorphOS](https://www.usenix.org/system/files/osdi18-khawaja.pdf)

## Questions




- Why does gVirt uses a fixed and large (16ms) time slice to schedule
  virtual GPUs?
  - 16ms or roughly 1/63 of a second is a nearly imperceptible change to humans,
  in image rendering, thus it is a reasonable value to use in the context of
  real-time GPU applications. Additionally, the cost of a context switch for the
  GPU is 3 orders of magnitude larger than a CPU, a larger time slice helps
  reduce the compute-context switch overhead ratio.
- Name at least one place where virtualizing GPU memory is different
  from virtualizing CPU memory.
  - while CPU memory virtualization generally present a virtual address space
  that is assisted by hardware paging mechanisms, GPUs cannot exercise such a
  system, thus the authors of gVirt partition the memory between guests instead.
- What makes it hard to space multiplex FPGA? (bonus point for an
  additional question of "what makes it hard to time multiple
  (dynamically schedule) FPGA?"
  - space multiplexing an FPGA is a difficult problem because the
  place-and-route portion of FPGA compilation process is long and
  time-consuming. Additionally, even if the placement between two fpga
  applications was interchangable, it still takes on the order of 10s-100s of
  milliseconds to reconfigure the FPGA with new logic. Time slicing applications
  on an FPGA is even more difficult because of the lack of an interface with
  which the scheduler can capture the state of a current FPGA applications
  before preempting it. With FPGAs the way state is stored is entirely different
  from a processor, so there is no good common interface for which applications
  can be preempted by time-sliced scheduling.


## Summary

### A Full GPU Virtualization Solution with Mediated Pass-Through

At the time of writing this article, GPUs were unable to be virtualized, meaning
that to enable a VM to use a GPU it had to have unfettered access to the device.
Another VM nor even the host or hypervisor could have access to the GPU. gVirt,
the solution proposed by the authors is a virtualization solution which allows
scheduling and time slicing of GPU compute and memory resources so that it can
be shared by multiple VMs enabling higher utilization in virtualized
environments.


### Sharing, protection, and Compatibility for Reconfigurable Fabric with AmorphOS

FPGAs can be taken advantage of to drastically increase the performance of
computing systems, but suffer from their lack of ability to share applications.
The authors of the paper introduce AmorphOS to space-multiplex GPU resources by
allowing users to configure "morphlets" which allows them to then be placed and
routed by the AmorphOS system to allow sharing of the GPU. The system maintains
a "high throughput" and "low latency" mode which changes how the FPGA is shared
depending on the performance targets.


