---
title: First-Generation Memory Disaggregation for Cloud Platforms
type: page
conf: arXiv
year: 2022
link: https://arxiv.org/pdf/2203.00241.pdf
---


MSFT First-gen CXL system - Basilisk

abstract

- argument: 25% of memory stranded
- 50% of all VMs never touch 50% of their memory

intro

- DRAM is 50% cost for azure [1]
- Google. only 40% of DRAM is used [9,10]
- stranding is when all cores are rented, but memory is not
- challenges w/ introducing memory disagg
	- customer intertia (no workload changes)
	- compatible with virtualization acceleration
	- commodity HW (why?, no good reasoning)

- larger pools --> higher latency (is this true?)
- contributions:
	- HW layer: memory controller, 8-32 socket connection
		- each socket has local mem + access to 1-NUMA hop mem pool
	- system software
		 - memory management layer, exposing memory to a VM OS as zNUMA (similar to cpu-less NUMA [54])
	- distributed SW layer?
		- relies on predictions of VM latency sensitivity

- performance goals:
	- VM performance degredation
	- resource efficiency
	- "small blast radius" --> minimize impact of failures
- feature requirements
	- customer inertia
	- compatibility
		- virtualization makes things difficult
		- VM memory in DC must have entire address range pinned
		- RDMA-based systems wouldn't work because can't rely on hypervisor page faults
		- cannot migrate pages between local/pool either
	- commodity HW

-
