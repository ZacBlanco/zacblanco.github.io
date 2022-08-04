---
title: ORION and the Three Rights; Sizing, Bundling, and Prewarming for Serverless DAGs
type: page
conf: osdi
year: 2022
link: https://www.usenix.org/system/files/osdi22-mahgoub.pdf
---


- Goal: probabilistic guarantees for E2E latency
- 3 challenges:
	- high variability+coorelation in execution of individual functions
	- skew in execution of parallel invocations
	- incidence of cold starts


- 3 features of the paper
	- performance model for runtime variability
	- method to co-locate parallel invocations
	- prewarming method



DAG workload characterization (Azure DF):
	width/depth/exec time:
		- median 3 stages
		- P95 depth of 8 stages
		- 65% of DAGs are linear chains
		- width is 37 in P95
		- time from 10ms-112 min, median 3.7s

	- DAG invocation freq/cold start:
		- DAG invocation heavily skewed
			- top 5 DAGs account for 46% of all invocations ( :O )
		- DAGs invoked > 100 times/day have very low cold start freq.
		- most DAGs rarely invoked
			- 80% of DAGs invoked < 100 times /day
			- high % of cold start - median 50%
	- variance in execution time
		- high skew ratios between application invocations
		- e.g. one app has P95 as 80X the P25
		- skew of execution times increases w/ more paralelization
			- skew is 2X for 98.2% of functions


latency modelling
	- model initialization and execution as separate distributions
	- series and parallel correlations
right-sizing
	- build a model of vm-size to latency distribution
	- 5-6 regions ([min, 1024], [1024, 1768], [1768, max), etc )approximately models the distribution
bundling parallel invocations
	- reduce total E2E latency

prewarming
	- optimization function to minimize E2E latency subject to constraint that utilization of given vector is > some target utilization
	- metric of utilization is estimated as (BusyTime/(busyTime+IdleTime))
	- BusyTime = Init + Exec time
	- Use Best-First Search and add 100ms to delay d_i yielding the highest improvement in utilzation without increasing E2E latency
	- algorithm terminates when adding delta to any delay factor does not improve utilization but increases E2E latency

-
