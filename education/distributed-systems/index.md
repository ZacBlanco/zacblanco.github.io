---
layout: page
title: Distributed Systems
permalink: /education/distributed-systems/
keywords: rutgers, pxk, pk, paul, distributed, systems, exams, notes, study, guide
description: A page with notes and other useful information from the Distributed Systems course at Rutgers
mathjax: true
---

**Note**: This course was taken during the fall semester of 2017

## Sections

- [Exam 2 Notes](#exam-2)
- [Exam 3 Notes](#exam-3)

## Exam 2 Notes <a id="exam-2"></a>

### Mutual Exclusion

**Mututal Exclusion** (Mutex) - Is when we give processes the ability to be granted exclusive access to a certain resource. 

Algorithms which provides mutex should be designed to avoid process startvation and that exactly one process gets the resource even if multiple request the resource concurrently.

The algorithms should be _fair_ in that every process which requests a resource will eventually get a chance to have it. If this does not occur we refer to it as **starvation**

Another name for a  mutex is a **lock** - because it locks out other resources. If there is a time limit on the lock then we call it a **lease**.

Categories of mutex algorithms:

- Centralized (uses central coordinator). Coordinator accepts a REQUEST, and responds with a GRANT. Client sends RELEASE when it is finished.
- Token-Based: Allows a process to access a resource if it is holding a special token. All of the process in the group create a communication ring network among all processes communicating. If a process receives a token and does not need the resource, the token is passed on. This ensures that eventually the token will reach around the ring and everyone will get a chance to hold the resource.
- Contention-Based: All process coordinate on who gets access to a specific resource.

**Lamport's Mutex Algorithm**: Require a process who wants a resource to send a timestampted request for the resource to _all_ processes in the group (including itslef). Immediately acknowledge any of these messages which are received and place the request within a priority queue sorted by lamport timestamp.

Basically the entire process involves broadcasting your request so that all members can construct the same locking queue whereby when you need to request lock access simply check if your request is at the top of the queue. _Based on reliable multicasts_

**Ricart and Agrawala**: A process which wants to access a resource must send a message to all other processes and wait for responses. If the process has the resource, delay response until you've let go of the resource. Respond to the process with the leaste recent timestamp.

### Election Algorithms

The goal of election algorithms is to get exactly one process selected as the "leader" of a group of other processes.

**Bully**: Selects the largest ID as the leader/coordinator.
  - Detect missing coordinator
  - Send ELECTION to all processes with higher ID
  - No messages received, elect self as coordinator
  - when process receives ELECTION send a response back immediately to the requestor and holds another election
  
**Ring Election**:

  - uses a logical communication ring among processes.
  - When detecting a missing coordinator, create ELECTION message containing PID and send to next process ring.
  - When receiving ELECTION, add PID to the body of the message and forward that to its neighboring process.
  When the message finally gets back to the sender, the original process looks at the list of active process and picks a coordinator from there.
  - Selecting highest/lowest process ID works because multiple elections may start in parallel.
  
**Chang and Roberts**

  - Improves upon the original ring algorithms
  - No need to keep list of all processes.
  - Simply pass the single message around and if one of the processes receives the message and has a larger process ID, replace the ID currently in the message and then pass the message along.

  - Another improvement is to check if the current node has participated in an election - and if so, drop any new election messages.

  - One problem with this approach is that when a network gets partitioned, the system may fail to elect a leader, in which case each segment must elect its own.
  
  - Another way to deal with segmentation is to use a quorum to perform operations, where as long as 50% of processes are available, operations may occur
  
### Consensus

**goal**: Have a group of processes agree on a common value

Consensus must satisfy **four goals**:

- Validity
- Uniform agreement
- Integrity
- Progress

Two main consensus algorithms:

- Paxos
  - Types of processes:
    - **proposers**: receive requests from clients. Responsible for running the protocol by sending messages to and receiving from acceptors.
    - **acceptors**: provide basis of fault tolerance.
    - **learners**:

    A majority of acceptors must be alive for paxos to properly function.
    
    Paxos:
    
    1.) Prepare: - proposer sends client message with unique proposal number
    
    2.) Accept: - After sending proposal to acceptors, the proposer must select the highest numbered proposal that it received and send and ACCEPT message to the acceptors with the information about the current highest proposal number to propagate.
    
To fix the issue of too many proposers with requests
    
- Raft
  - all client requests in raft must be routed through to the leader of the group/system
  - The main problem in raft is the elections.
  - Nodes can be **follower**, **leaders** or **candidate**.
  - All servers start in a **follower** state.
  - **Leaders** send periodic heartbeats.
    - If a follower times out (doesn't get a heartbeat) it will instantiate a new election and set its role as **candidate**
    - The followers pick a random time interval before making itself a candidate, then votes for itself and sends a request vote to all servers.
    - If a server sends a request vote and has not voted, it will vote for the new candidate.
    - Terms are marked by incrementing numbers
  - In order to perform consensus with raft a request must be routed to the leader.
    - The leader sends the uncommitted message to all followers, and when it receives repsonses from a majority of followers it will send the COMMIT message to the rest of the followers at which point the new entry will be committed.

### Locks vs Leases

Lease is a lock w/ expiration time.

### Distributed Transactions

- ACID Transactions
  - Atomic
  - Consistent
  - Isolated
  - Durable

- A write-ahead log is used to enable rollback to previous transactions (see consensus for committing distributed log entries.)

### Commit Protocols

#### Two-Phase Commit Protocol

Phase 1:

- Every member of the group responds to the coordinator

Phase 2:

- The coordinator sends the commit or abort order and waits for a response from everyone
- Doesn't handle dying nodes.


#### Three-Phase Commit Protocol

Three phase is similar to two phase except it allows timeouts

Phase 1:

- Query - can you commit?

Phase 2: 

- Send "agreed to commit" message.

Phase 3:

- Send "commit ___" message

Three phase allows 2 things:

1. Enables the use of a recovery coordinator
2. Every phase can now time out, instead of indefinitely waiting.

### CAP Theorem

You can only have two of:

- Consistency
- Availability
- Partition Tolerance

Cousin of ACID sometimes we want **BASE**

- Basic Availability, Soft-state, Eventual Consistency
  
### Concurrency Control

**Goal**: Allow multiple transactions to run concrruently but ensure that any resource access is controlled to give results which are identical if they ran serially. Perserves the Isolated gurantee of ACID.

- Accomplish via exclusive locks and a lock manager.
- Growing/shrinking ==> Locks acquireed/released
- Exclusive locks are aggressive, think read/write locks instead.

- Working phase
- Validation phase
- Update phase

### Distributed Deadlock

- **Goal**: Determine if a transcation will wait for resources in such a manner as to create an indefinite wait, called deadlock - find ways to ensure this won't happen.

- Accomplished via:
  - mutex
  - hold and wait
  - non-preemption
  - circular wait
 
**Detection**:

- Centralized w/ a coordinator (wait-for graph)

### NAS

- NFS
  - Stateless RPC-based read/write/link/mkdir/rm
  - works well in faulty environments.
  - Reads in blocks (8KB default)
  - Uses validation by comparing modification times
  - File locking not supported at first
  - Automounter mounts remote directories only when first accessed to make keeping track of pointers easier.
- AFS
  - Improve NFS on massive scale
  - NFS suffered b/c client would never cache data
  - Cached large amounts of data for a long time (whole-file and long-term caching)
  - Provides session semantics (last to open/close gets the write)
  - client never bothered server when cached.
  - Server keeps track of clients (callback promise)
  - Server goes to list of promises and asks callbacks from each client.
  - Files are shared in "units" - called volumes
- Coda
  - Built on AFS
  - Support replicated servers and disconnected operation
  - Abstract volume to VSG (volume storage group)
  - Accessible VSG for volumes a client can currently access.
  - If no servers are available, client goes into disocnnected operation mode, all changes logged to the CML.
  - Upon connection changelog is sent.
  - Support for hoarding (user-directed caching)
- DFS (AFS, v3)
  - Primary design goal is to acoid unpredictablelost data w/ session semantics and multiple clients
  - concept of tokens is introduced to perform operations on a file's data.
  - open, data, status, lock tokens
  - tokens are granted and revoked by the server (given to clients)
- SMB/CIFS
  - Server Message Block protocol
  - Connection oriented stateful filesys w/ a priority on consistency and lock support rather than caching and performance.
  - OPLOCKS (Opportunistic Locks), modified version of DFS tokens - oplocks tell the client how it may cache data
    - Oplocks can be revoked or changed by the server.
  - Eight separate oplocks:
    - Level 1 (Exclusive oplock) - exclusive rw access
    - Level 2 (Shared oplock) - Granted in cases where multiple processes may read a file and no processes are writing to it
    - Batch oplock - Another exlusive type of oplocks, allows a client to keep a file open even if the server process closed the file. optimizes the case where a process repeatedly opens and closes the same files.
    - filter oplock - exclusive and allows applications that hold oplock be preempted whenever other processes need to access the file.
    - Read oplock - shared oplock which is the same as level 2 - supports read caching
    - rw oplock gives client exclusive access and read-write caching.
    - rwh oplock enables a client to keep a file open on the server even if the client closes it - 
- SMB2+
  - reduced complexity
  - 100 to 19 commands
  - pipelining (aend multiple commands) - credit based flow control
    - multiple commands w/o a response
  - compounding - multiple commands in one message
  - large r/w sizes
  - improved caching
  - durable handles
- NFS v4
  - NFS version 4 added state to the NDS server. Control client side and cache
  - Support for compound RPC into single request
  - Additions for authentication and encryption

### Chubby (Apache ZooKeeper)


- **Goal**: Highly available and centralized lease manager and file system for small files that can serve as a nameserver and configuration repository

- Defaults to running on five nodes as five active replicas. Called a **chubby cell**
- Only the master seres client requests
- Majority must be running - paxos is used to elect the leader and keep replicas consistent
  - typically one chubby cell per datacenter
- Designed for coarse-grained locks. (large sets of files, or a whole table)
  - Not fine-grained (single files/rows)
- Data written to disk when modified.
- DB backed up on GFS

### Parallel File Systems

- **Goal**: Store huge files (terabyte and larger) in a file system that is distributed across many (thousands) of machines

- **GFS**:
  - GFS has one master file server. Faster server is supposed to be more reliable and manages metadata of the filesystem. Actual data does not reside here.
  - Chunks are typically 64MB, stored on chunkservers
  - to write, chunk master grants a chunk lease to one of the replicated chunks. - called the **primary replica chunkserver**
    - The replica then forwards the data to another replica, that one to another, and so on.
    - Avoids single server sending N copies of the data.
    - Once replicas acknowledge the receipt of the data, the actual writing takes place. Clients send WRITE request to primary chunkserver which identifies the data which was sent. The primary assigns a sequence to the write operations which take place so that they all write data in the proper order.
- **HDFS**:
  - Apache HDFS uses a namenode to store all master data.
  - typical blocks are 128MB by default, alll data is stored on datanodes. Each block is replicated on multiple datanodes (default 3)
  - Uses rack-aware logic for determining which computers should host replicas of a block.

### Distributed Lookup (Hash Tables)

- **Goal**: Create a highly-scalable, completely decentralized KV store.
  


## Exam 3 Notes <a id="exam-3"></a>

### Google MapReduce (and Apache Hadoop)

Traditional programming is typically serial

- In parallel programming we break the data into chunks in which the processing can be run concurrently across multiple cores or machines.

The challenge with this is that we need to identify which tasks can be run concurrently and which might be serial. Not all problems may be parallelized

In the simplest case there is **no dependency** among the data - meaning that data can be split into equal sized chunks and processed.

- Simplest approach is master-worker
  - Master initializes arrays and splits according to # of workers
  - Worker receives subarray and performs processing, send results to master

One of the first widely user frameworks to accomplish general-purpose paralleization was **MapReduce**

- Created by google in 2004, open sourced later.

MapReduce is based on two distinct functions - `map` and `reduce`

- `map(f, data)` applies a function `f` to each `data`
- `reduce(f, data)` combines values together using some kind of 2-to-1 function. i.e. addition

Google's MapReduce was great because it

- Gave programmers a simple, standard API where they don't need to worry about
  - parallelization
  - data distribution
  - load balancing
  - fault tolerance
- Great because we can process _huge_ (petabyte scale) amounts of data over thousands of processors

**Implementation**:

- Google
- Apache Hadoop
- Amazon Elastic MapReduce (EMR) using Amazon EC2


Map (in MapReduce) splits data in **shards** (process known as partitioning) where the shards are processed on a single machine. It then aggregates the values and sorts them in an intermediate phase before passing to reduce.

Reduce then takes the sorted intermediate phase K/V pairs and aggregates them into some kind of result. The K/V pairs are mapped to nodes via a paritioning using a hash function (`hash(key) mod R`, where R is the number of partitions)

So, in all we go follow:

> `Input files > shards > map workers > intermediate files > reduce workers > output files`

Within the map worker there are two functions

- Map, perform function all all dat ain the shard
- Partition, identify which of the reducers will handle the keys, We force the data to move to one of the reduce workers based on the partitioning function.

Within the reduce worker there are two functions:

- Sort, which fetches the relecent partitions from the output of all mappers, and sorts the keys in order
- Reduce takes the sorted output and calls the reducer's aggregation function on the data.


In detail, the steps are as follows:

1. Split input files in chunks (typically around 64MB)
2. Fork processes/start up copies of workers on many machines
  - 1 master scheduler and coordinator
  - Idle workers are assigned either a map or reduce task
3. Map task
  - Read contents of input shards assigned
  - Parses KV pairs
  - Passes pair to a user defined map function
4. Create intermediate files
  - Intermediate files buffered in memory, periodically written to the local disk
    - Partitioned in R regions
  - Notifies master when complete.
  - 4a. Map data processed by the reduce workers
     - All KV pairs are SORTED
     - Parition function decides which of R reducer worker will work on which key
     - each reduce worker reads partition from EVERY map worker
5. Reduce task: sorting
  - Reduce workers are notified by master about location of intermediate files for its partition
  - Uses RPCs to read data from disks of map workers
  - When reading the data, the reduce worker sorts is by the intermediate key and occurrences of the same key are grouped together.
6. Reduce Task: reducing
  - The reduce function is given a key and set of values for a key, it then writes the results to a file.
7. Return data to the user

**Fault Tolerance**:

- The master will ping workers periodically
  - If no response is received within a timeframe, the worker is marked as failed.
  - Tasks given to the worker are reset and initialized to the prior state

**Locality**

- Input and output files are on GFS (or HDFS), The master tries to schedule map workers on the machines that has a copy of the input chunk that it needs.

#### Examples

**Distributed `grep`**

- Map: emits a line if it matches a pattern
- Reduce: copy the intermediate data to output

**Count URL Access Frequency**

- Map: process logs of webpages, emit `<URL, 1>`
- Reduce: Add values of same URL

**Reverse web-link graph**

- Map: output `<target, source>`
- Reduce: concatenate the list of all source URLs associated with a target

**Inverted Index**

- Map: parse document, emit `<word, document-ID>`
- Reduce: For each word, sort corresponding document IDs

As great as MapReduce is, all is not perfect. MapReduce used _batch processing_ and took _hours_ to run.

- The web has become much more dynamic and the graph needs to be refreshed in seconds for some sites.

In practice, data is also typically not simple files. It is stored as B-Trees, tables, databases, memory mapped KV pairs

Textual data is also rarely used. Google's protobuf is one widely used framework to convert textual data/objects into binary formats.

### BigTable (and Apache HBase)

BigTable is a Highly available distributed storage system for _semi-structured_ data. i.e

- URLs: content, metadata, links, achord, PageRank
- User data: preferences, account info, recent queries
- Geography: roads, satellite images, interest points, annotations

BigTable runs on a large scale (petabytes). It can store billions of URLs with many versions on each page, hundreds of millions of clients, thousands of queries per second. It stores 100+TB of imagery

It is used for many google services.

It is **NOT** a relational database like typical SQL databases. (Though it does _appear_ as a table like many traditional databases)

- BigTable is sparse, distributed, persistent multidimensional sorted map.


A _simple_ example of how a table is structured is shown below

|         | col 2 | col 3          |
|key 1    |  t1,t2| t1, t2, t3, t4 | 
|key 2    | t2, t3| t1|

Where in each cell, data at different timestampts, t1, t2, t3, etc.. are stored.

- All row operations are **atomic**
- Tables are partitioned dynamically by rows into **tablets**
- **Tablet**: A range of contiguous rows
  - Unit of ditribution and load balancing
  - Nearby rows will be served by same server
  - Accessing nearby rows required communication with a small amount of machines
  - Select good row keys to ensure locality. i.e. reverse domain names.

#### Table Splitting

Tables in BigTable start as a single tablet, as it grows, it will split into multiple tablets. I.e. it selects a split point and puts the rows before the point into a tablet, and after the same point into another tablet.

**Column Families**: group of column keys, the basic unit of data access

- Data in column families is typically of the same time.
- Implementation compresses data in the same column family
- You can either (1) create a column family or (2) store data in any key within the column family
- Families are typically small
  - Less than or equal to a few hundred keys, a table may have an unlimited number of column families.
  - Identified by `family:qualifier`

**Timestamps**

- Each column family may contain multiple versions
- version indexed by 64-bit timestamp
- Per column-family settings for garbage collection
  - Something like keep only latest $$n$$ versions
  - Keep versions written since time $$t$$
- Retrieve most recent version if no version is specified

#### API Operations

- create/delete tables & column families
- change cluster, table, and column family metadata (ACLs)
- Write or delete values in cells
- read values from specific rows
- Iterate over subset of data in a table
  - all members of col. family
  - multiple col. families
  - multiple timestamps
  - multiple rows
- Atomic ready-modify-write row
  - This is important for CAP

The implementation of BigTable is supported by

- GFS, the distributed google file system
- cluster management system (scheduling jobs, monitoring health, dealing w/ failures)
- Google SSTable (Sorted String Table)
  - Internal file format which is optimizd for streaming I/O and storing KV data
  - provides persistent, ordered, immutable map from keys to values
    - append only
  - memory or disk based, indexes are cached in memory
  - if there are additions, deletions, or changes to rows
    - New SSTable are written out w/ deleted data removed
    - Periodic compaction merged SSTables and removed old and retired ones.
- Chubby, googles distributed locking service
  - Highly available
  - five active replicas, one as master to serve requests
  - majority must be running
  - paxos used here
  - provides namespace of files and directories
  - Ensures there is only one active master
  - store bootstrap location of bigtable data
  - discover tablet servers
  - store bigtable schema information
  - store access control lists.

Bigtable is implemented using many **tablet servers** - these coordinate requests to tablets.

- can be added or removed dynamically
- Manages a set of tablets (10-10k tablets/server)
- Handles r/w requests to tablets
- splits tablets when too large

BigTable assigns one tablet server as a **master**. It

- Assigns tablets to tablet servers
- balances tablet server load
- handles garbage collection of unneeded files in GFS
- schema changes (table & family column creation)

BigTable also has a **METADATA** table which stores information about bigtable itself

- It is a three-tier table
- Balanced structure similar to a B+ tree
- Root tablet contains location of all tablets in a special METADATA table
- Row key of METADATA table contains the location of each tablet
- $$f(\text{table_ID, end_row}) \rightarrow \text{location of tablet}$$


Every tablet is assigned to one tablet server at a time.

Chubby is responsible for keeping track of the tablet servers. It creates and acquires a lock on a uniquely named file in a chubby server directory. The master monitors the directory to discover new tablets.

When the master starts:

- Grabs a unique master lock in chubby
- scans the servers directory
- contacts tablet servers to discover what tablets are assigned to that server
- scans the METADATA table to learn full sets of tablets
  - Builds a lists of tablets not assigned to servers
  - Assigned by choosing a tablet server and sending it a `tablet_load` request

#### Fault Tolerance

- Fault tolerance provided by GFS and Chubby
- In the case of a dead tablet server, the master is responsible for detecting when a tablet server is not working
  - Asks tablet servers for lock statuses
  - If tablet server cannot be reached or has lost its lock:
    - Master attempts to grab the server's lock
    - If it succeeds then the tablet server is dead or it cannot reach chubby
    - Master moves tablets assigned to that server into unassigned state.
- Dead master
  - Master kills itself when its chubby lease expires
  - Cluster management system detects a non-responding master
- Chubby designed for fault tolerance
- GFS stores underlying data (n-way replication)
- Uses eventual consistency

#### BigTable vs Dynamo

- Dynamo targets apps that only need KV access with a primary focus on high availability
  - KV store vs column store
  - Bigtable: Distributed DB built on GFS
  - Dynamo: Distributed hash table
  - Updates are not rejected during network partitions or server failures.

### Google Spanner

- Successor to BigTable (kind of)

Spanner adds:

- Familiar SQL-like multi-table row-column data model
  - One primary key per table
- Synchronous replication (Bigtable was eventually consistent)
- Transaction across arbitrary rows

Generally, Spanner is

-  Globally distributed multi-version database
- ACID (general purpose transactions)
- Schematized tables
  - Built on top of a key-value based implementation
  - SQL-like queries
- Lock-free distributed read transactions

The goal of spanner is to make BigTable easy for programmers to use

Working with eventual consistency and merging is hard _don't make developers deal with it!_

#### Data Storage in Spanner

- Tablets are sharded across rows into tablets
- Tablets are stored in **spanservers**
- 1000s of spanservers per zone
  - Collection of servers which can be run independently
- **Zonemaster** allocates data to spanservers
- **location proxies** are used by clients to locate spansrvers which hold the necessary data
- **universemaster** holds the status of all zones
- **placement driver** transfers data between zones

The **universe** holds one or more databases

The **databases** hold one or more tables, where tables are an arbitrary number of rows and columns

  - Storage may be interleaves
  - all data in a table still has version information

Shards and tablets are replicated synchronously with Paxos and transactions across shards use the 2-phase commit protocol

A **directory** is a set of contiguous keys which is used as a unit of data allocation and provides **granularity** for data movement between Paxos groups.

#### Transactions

All transactions follow ACID and follow **strict 2-phase locking**.

1. Acquires all locks
2. Do work
3. Get a commit timestamp
4. Log the commit timestamp via paxos to a majority of replicas
5. do the commit
  - Apply changes locally and to repicas
6. release the locks

However, 2-phase locking can be slow, so we use separate read and write locks.

- **R** locks block behind **W** locks
- **W** locks block behind **R** locks

**Multiversion concurrency** saved us by taking a snapshot of the database for transactions up to a point in time. It then allows us to read old data without getting a lock (good for long-running reads, i.e. searching).

This means we **must** have commit timestamps that enable meaningful snapshots.

#### Getting Good Commit Timestamps

- Vector clocks can work
  - Pass server's notion of time with each message
  - Receiver updates time if necessary
- Not feasible for large systems
- Doesn't work for things like phone call logs or with HTML
- **Spanner's solution** - use _physical timestamps_.
  - If $$T_1$$ commits before $$T_2$$, then $$T_1$$ _must_ get a smaller timestamp
  - Commit order matches global wall-time order.

**TrueTime**

- Because we can't know global time across servers
  - **Global wall-clock time** = time + interval of uncertainty
    - `TT.now().earliest` = time guaranteed to be less than or equal to current time
    - `TT.now().latest` = time guranteed to be greater than or equal to current time.
  - Each data center has a GPS receiver and atomic clock
  - Atomic clock synchronized with GPS receivers
    - Validates GPS receivers
  - Spanservers periodically synchronize with time servers
    - Know unvertainty based on interval
    - Synchronize every 30 seconds: clock uncertainty < 10ms

**Commit Wait**

- We don't know exact time - but we **can** wait out the uncertainty

1. Acquire locks
2. Do work
3. Get a commit timestamp `t = TT.now().latest()`
4. Commit WAIT: until `TT.now().earliest > t`
5. Commit
6. Release locks

If we want to integrate data replication and concurrency control we can create the replicas of our data while holding the **Commit wait**

#### Spanner Conclusion

- ACID Semantics are not sacrificed
  - Life gets easier for programmers
  - Don't need to deal w/ eventual consistency
- Wide-area distributed transactions built-in
  - Bigtable did not support distributed transactions
  - programmers needed to write their own
  - Easier if programmer don't need to get 2-phase commit correct
- Clock incertainty is known to programmers.

### Other Parallel Frameworks

#### Apache Pig

- With pig it makes MapReduce much easier to use rather than java.
- Built-ins for common operations such as join group filter

Used via **grunt**, the Pig shell. It allows you to submit scripts directly to Pig. Pig then compiles to MapReduce jobs.

Generally in the Pig framework (written in PigLatin):

1. submit script
2. Pig framework:
  - Parses, checks, optimized, plans execution, submit jar to hadoop, monitors progress
3. Hadoop executes Map and Reduce functions.

Pig loads data via PigStorage, BinStorage, BinaryStorage, Textloader, or PigDump

However, while mapreduce is a great parallel framework, is doesn't scale well for all problems.

For many other problems it will require multiple iterations and the data locality is typically not preserved between Map and Reduce jobs. This means there is  *lots* of communication between map and reduce workers.

### Bulk Synchronous Parallel (BSP)

BSP is computing model for parallel computations.

- Built via a series of supersteps.
  - Initial data
  - compute on each
  - Shuffle into barrier stage (wait for all computes)
  - Rearrange messages for next superstep (outputs become inputs)

**Concurrent Computation**

- In BSP, processes are randomly assigned to different processors.
- Each process uses only local data, computation is asynchronous
- Computation times may vary

**Communication**

- Messaging is restricted to only the end of a computation superstep
- Each worker sends a message to 0 or more workers
- Messages are the input for the next superstep.

**Barrier Synchronization**

- The next superstep does not begin until all messages have been received
- Barriers ensure no deadlock: no circular dependencies can be created
- Provides an opportunity to checkpoint results for fault tolerance
  - i.e. restart computation at last successful superstep

**BSP Implementation**: _Apache Hama_

- Hama provides a BSP framework on top of Apache Hadoop HDFS
  - Provides automatic parallelization and distribution
  - Uses hadoop RPC
    - Data serialized with google protobuf
  - Zookeeper used for coordination
    - Handles barrier sync notifications
- Good for applications with data locality
  - Matrices and graphs
  - Algorithms that require many iterations

**Hama Programming**

- Preprocessing
  - Define the number of peers
  - Split initial inputs for each of peers
  - Framework assigns unique ID to each worker
- Superstep: the worker function is the superstep
  - `getCurrentMssage()` - input messages from previous step
  - `compute` - user code
  - `send(peer, msg)` - send messages to a peer
  - `sync()` - synchronize with other peers (barrier)
- File I/O
  - KV model used by hadoop and MapReduce & HBase/BigTable
  - `readNext(key, value)` 
  - `write(key, value)`

### Graph Computing

Examples:

- Social links
- web pages
- network connectivity
- roads
- disease outbreaks

Graph computation is a difficult problem on a large scale

- Computation with graphs:
  - poor locality
  - little work per vertex
- Distribution across machines
  - Communication complexity
  - Failure concerns
- Solutions
  - Application-specific, custom solutions
  - MapReduce or databases
    - May require many iterations (and lots of data movement)
  - Single computer libraries: limits the scale
  - Parallel libraries do not address fault tolerance
  - BSP is OK but too general


#### Pregel: A vertex-centric BSP (Apache Giraph)

- Input: a directed graph
  - Each vertex is an object
    - vertex has unique name
    - vertex has a modifiable value
  - Directed edges: links to other objects
    - Associated source vertex
    - Each edge has a modifiable value
    - Each edge has a target vertex identifier  
- Computation: A series of supersteps
  - Same user-defined function run on ever vertex
    - receives messages sent from previous superstep
    - May modify the state of the vertex or of its outgoing edges
    - Sends messages that will be received in the next superstep
      - typically to outgoing edges
      - can be sent to any known vertex
    - May modify graph topology
  - Every superstep ends with a barrier synchronization
- Termination
  - Pregel terminates when every vertex votes to halt
  - initially every vertex is **acive**
    - Active vertices compute during a superstep
  - Each verte may choose to deactivate itself by voting to halt
    - Vertex has no more work to do
    - Will not be executed by pregel
    - UNLESS the vertex receives a message
      - receiving a message reactivates
      - Will stay active until a vote to halt.
  - Algorithm will terminate when all vertices are inactive and there are no messages in transit
- Output
  - Output is the set of values output by the vertices
  - often a directed graph
    - may be non-isomorphic to original since edges and vertices can be added or deleted

More examples of graph computations:

- Shortest path to a node
- Cluster identification
- Graph mining
- PageRank

**Pregel Locality**

- Vertices and edges remain on the machine that does the computation
- To run in mapreduce requires chaining multple mapreduce operations and the entire graph state is passes from Map to Reduce, over and over again.

**Pregel API**

- `Compute(MessageIterator)` - Executed per active vertex
- `GetValue()` - gets current vertex value
- `MutableValue()` - set current vertex value
- `GetOutEdgeIterator()` - gets the list of outgoing eges
  - `.target()` - Identify target vertex of an edge
  - `.GetValue()` -get edge value
  - `MutableValue()` - set edge value
- `SendMessageTo()`
  - Any number of messages may be sent
  - No guaranteed ordering
  - Message may be sent to any vertex, but we must have its ID

Advanced Pregel API operations

- **Combiners**
  - Each message has an overhead - so try to reduce the number of messages
    - many vertices are processed per worker (multit-threaded)
  - Combiners are application specific
    - Programmer subclasses a combiner class and overides the `combine()` method
  - No gurantee on which messages may be combined.
- **Aggregators**:
  - Handle global data
  - A vertex can provide a value to an aggregator during a superstep
    - Aggregators combine received values to one value
    - Value is available to all vertices in the next superstep
  - User subclasses an `Aggregator` class
- **Topology Modification**:
  - or clustering - combine two vertices into one vertex
  - i.e. if computing  spanning tree - remove unneeded edges
  - Add or remove edges and ertices
  - Modification will be visible in the next superstep.

To run Pregel, many copies of the program must be started within the cluster of machines

One copy of the program becomes the **master**

  - This program will not be assigned to the graph and is responsible for coordination

The cluster's nameserver uses chubby

- **master** registers itself wilth the chubby nameservice
- Workers contact the nameservice to find the master

- **Partition Assignment**:
  - Master determines the number of partitions
  - One or more partitions are assigned to each worker
  - By putting more than 1 partition per worker it improves load balancing
  - Workers are responsible for its section of the graph
  - Each worker knows the vertex assignments of other workers.
**Input Assignment**:
  - Master assigns parts of the input to each worker
    - Data typically in GFS or BigTable/ HDFS/HBase
  - Input = a set of records
    - Record = vertex and edges
    - Assignments are based on file boundaries
  - Workers read the input
    - Worker will read input and pass to vertex processor, else send the data to the appropriate worker.
    - After data is loaded, all vertices are marked as **active**
- **Computation**
  - Master tells each worker to perform superstep
  - Worker
    - Iterates through vertices
    - Calls compute for each vertex
    - Delivers messages
    - Outgoing messages sent asynchronously
  - When finished, worker tells master how many vertices are active in the next superstep
  - Computation done when no more active vertices in the cluster.
- **Handling Failures**
  - _Checkpointing_
    - Controlled by master every N supersteps
    - Master asks a worker to checkpoint
      - Save state of partitions (Vert values, edge values, incoming messages)
  - Master sends a **ping** messages to workers
    - If a worker doesn't respond, then the worker is marked as failed
    - If a worker doesn't receive a ping within a timeframe, it terminates.
  - When failure is detected
    - Master reassigns partitions of current worker set
    - All workers reload partition state from most recent checkpoint.

Apache Giraph is the Open Source version of Pregel

- Initially created at Yahoo
- Used at facebook to compute social graph
- Runs on mapreduce (Map Only job)
  - Uses java instead of C++

### Apache Spark - Generalizing MapReduce

- The goal of spark was to generalize MapReduce. It uses similar shard-and-gather approach like mapReduce.
- Adds fast data sharing and general DAGs

Spark uses a generic data storage interface

- Can use HDFS, Cassanda, HBase, or whatever you please
- Stores data as RDDs (Resilient distributed dataset)

Spark also uses amore general functional programming model which uses **transformations** and **actions**

With regards to MapReduce:

- _transformation_ = **map**
- _action_ = **reduce**

At a high level, Spark uses a DAG to produce execute jobs

- **Job** = a bunch of transformation and actions on an RDD

The **cluster manager** breaks a job into tasks which then sends **tasks** to **workers** where the data live.

**Worker Node**

- One or more **executors**
  - JVM process
  - Talks with cluster manager
  - Receives **tasks**
    - Task = transformation or action
  - Data to be processed is an RDD which is local to the node
  - Caches data
    - Stores frequently uses data in memory
    - Key to Spark's high performance

**Data and RDDs**

- RDDs can be created form any type of file, streaming sources, or from a spark transformation
- RDDs are:
  - Immutable
    - Cannot change
  - Typed
    - contain some kind of parsable data structure
  - Paritioned
    - parts of RDDs may go to different servers
  - Ordered

**Operations on RDDs**

- Transformations
  - Lazy - not evaluated immediately
  - transformed RDD is recomputed when an action is run
  - RDDs can be persisted into memory or disk
- Actions
  - Finalizing operations
    - Reduce, count, sample, write to file

Spark does not care how data is stored. The RDD connector determines how to read the data

RDDs are also fault tolerance and track the sequence of transformation used to create them. This enables the recomputing of lost data in the case of a failed node.

**Spark Streaming**

MapReduce and pregel expect static data. Spark streaming enables processing of live data streams. Data is chunked into batches and processed via the spark engine.

- This is accomplished via `DStreams` - continuous streams of data which are discretized and chunked.
- Appears as a continuous series of RDDs
- Each operation on DStream translates to operations on the chunked RDDs
- Join operations allow combining multiple streams.


### Content Delivery Networks

Serving web content from one location presents many problems. Scalability, reliability, and performance to name a few

We also have to deal with the flash crowd problem. _What if everyone comes to your site at one time?_

CDN's cache content and service requests from multiple servers at the network edge (close to the user)

This:

- Reduces demand on site's infrastructure
- Provide faster service to users
  - Content comes from nearby servers

CDNs focus on content

- Computing is still done by the site host's server(s)
- The other parts of the site (images, scripts, etc) which often make up the bulk of the data of a site are offloaded to the CDN.

Loading a webpage is similar to a memory hierarchhy where we have CPU Cache, RAM and Disk. To load a page browsers use a content cache, a CDN, and then finally the host's own web server.

One function that CDNs have is load balancing by increasing the numer of servers that can be reahed for content which offloads the work from going to a single web server.

**Multihoming**

- Gets the network links from multiple ISPs
- Server has one IP address but may have multiple links
- Announce address to upstream routers via BGP protocol.
  - Provides clients with a choice of routes and makes the network fault-tolerant.

**Mirroring**

- Used to synchronize multiple servers
- Useful for load balancing and fault tolerance

To improve internet scalability, availability and performance we can mirror servers for load balancing. This affects scalability and availability. Lastly, we can cache content and serve requests from multiple servers at the network ege which is close to the user

This reduces demand on site's infrastructure and provides faster service to users by reducing network latency (RTT)

Some of these approaches have problems

- Load balaning:
  - Data center or ISP can fail
- Multihoming
  - protocols like BGP are often not quick to find new routes
- Mirroring at multiple sites
  - synchronization is not an easy problem
- Proxy servers
  - Typically a client-side solution
  - Low cache-hit rates.

All of these require extra capcity and capital costs.

**Akamai Distributed Caching**

Akamai is a company which evolved from MIT research. It aimed at tackling the "flash crowd" problem. 

Delivers approximately 15-30% of all web traffic (over 30 terabits/second)

The goal of Akamai is to try to serve clients from servers which are likely to contain the necessary content. The nearest, most available, and likely server which could have the content the user is requesting.

Because the internet is a collection of many autonomous networks, connectivity is based on business decisions and peering agreements, not simply performance of the servers

- An ISPs top performance incentives are Last-mile connectivity to end-users
  - Connectivity to servers on the ISP

Akamai's overlay network is a collection of caching servers at many, many ISPs which all know about one another.

**Akaimai Overlay Network**

- DNS lookup
  - Translated by a mapping system to an edge server that can serve content
  - Uses custom DNS servers which takes the requestor's address into account to find the nearest edge
- Browser sends requests to given edge servers
  - Edge server may be able to serve content from its cache
  - May need to contact the origin server via the transport system.
- Mapping: Domain name lookup
  - Uses dynamic DNS servers
  - Resolve a host name based on:
    - user location
    - Server health
    - Server load
    - Net status
    - load balancing
  - Tries to find an edge server at the customer's ISP
- Collecting network performance data
  - helps map the network performance data
  - Based on BGP and `traceroute`
  - estimate hops and transit time
  - content server report their load to a monitoring application
    - App publishes load rpeorts to a local akamai DNS server
  - Akamai DNS server determines which IP addresses to return when resolving names
  - **Load Shedding**: when servers get too loaded, the dns server will not respond with those addresses.
- Benefits of the overlay CDN network
  - Caching
    - Increase hit rate on nearby servers
    - Static content server from caches
    - Hierarchical caching
    - Can be used with static
    - Some dynamic content with ESI (edge side includes)
    - Streaming media
  - Routing
    - Route to parent server or origin via overlay
    - routing decisions determined by measured latency, packet loss, and available bandwidth
    - Results in a ranked list of paths from edge to origin
    - Each intermediate node acts as a forwarder
    - Keeps TCP connections open for efficiency
  - Security
    - High capacity - mitigate DDoS attacks
    - Expertise
      - Maintain systems and software
    - hardened network stack
    - detects and defend against attacks
    - Shields the origin servers.

**Other uses for CDNs**

- Signed URLs in Amazon CloudFront
  - Similar concept to Akamai
  - Requests for content are routed to nearest edge location
  - Integrates easily w/ Amazon backend services
  - Private content:
    - Provide specific URLs for restricted content
    - control access to content via a signed URL
    - URL contains:
      - a policy or reference to a policy
      - Signature = encrypted hash
    - Policies include: 
      - validity: start and end time
      - Range of IP addresses that are allowed to access the object


**Limelight Orchestrate**

A service focused on video distribution and content management

- Video transcoding
  - Encode video to a variety of formats
  - support playback on various devices w/ different formats and bitrates
- Ad insertion
  - Integrate w/ doubleclick, liverail, tremor, YuMe
  - Pre roll post roll, mid roll, overlay, etc..
- Publish -> Transcode to servers -> universal URL
  - Transcodes are sent to content servers
  - Client accesses universal URL and gets content from a nearby content server

### Clusters, Cryptography, Authentication, and Authorization

#### Clusters

**Computer System Design**

- Highly Available Systems
  - Incorporate elements of fault tolerant design
  - Fully fault tolerant systems will offer non-stop availability
    - Really impossible to achieve
  - Increase in availability is directly correlated with the amount of money spent to achieve availability
- Highly Scalable Systems
  - SMP architecture
  - Problem:
    - Performance gain as # of processors increased is sublinear
      - contention for resource
      - expensive
- Clustering
  - Achieve reliability and scalability by interconnecting multiple independent systems
  - **Cluster**: a group of standard autonomous servers configured so they appear on the network as a single machine
  - Ideally clusters are off-the-shelf, interconnected on a LAN, appear as one system, processed are load balanced, and the system is fault tolerant
  - All of these are essentially _impossible_ to achieve (as of now) in a single package
- Types:
  - Supercomputing (HPC)
  - High Availability (HA)
  - Load Balancing
  - Storage
- Cluster Components
  - Cluster membership
  - quroum
  - configuration and storage management
  - interconnect
  - storage
  - heartbeat and heartbeat network
- Cluster Membership
  - Software to manage cluster membership
  - What nodes are in the cluster
  - Which are currently alive?
  - Seen in:
    - virtual synchrony
    - GFS master/HDFS namenode
    - Bigtable master
    - Pregel master
    - MapReduce and Spark master
- Quorum
  - Some members may be dead or disconnected
  - **Quorum**: numer of elements that must be online for the cluster to function
    - Voting algorithm to determine whether the set of nodes has a quorum
  - Keeping track of quorum requires us to count cluster nodes and seeing if at least half of them are active
  - Seen in Paxos and Raft
- Cluster Configuration and Service Management
  - Cluster configuration system
    - Manages configuration of systems and software in a cluster
    - Runs in each cluster node
      - changes propagate to all nodes
      - Admin has a single point of control
  - Service management
    - Identify which applications run where
    - Specify how failover occurs
      - Active: system runs a service
      - Standby: which sytems can run the service if the active dies?
    - Ex: MapReduce, Pregel, Spark all user coordinators
- Interconnect
  - Provide communication between nodes
  - Goal: Low latency and high bandwidth, low CPU overhead, and low cost
  - LANs are typically best
  - Current max switch capacities are about ~5Tbps
  - Datacenters need hundreds of Tbps if not Pbps
  - Switches however add latency
    - 1-8 $$\mu s$$ for a single switch
    - For 1-2 meters of cable, 5ns/m propagation delay time.
  - Between cluster racks, there is at least 3 switches (3-24 $$\mu s$$)
    - ~10-100 meters of cable (50-500ns delay)
  - Add normal latency of sending and receiving packets within the server to
    - 1KB packet is about 1 $$\mu s$$
  - Dedicated cluster interconnects
    - TCP adds latency
      - OS overhead, queueing, checksums, acks, congestion control, fragmentation, and reassembly
      - Lots of interrupts
      - Consumes time and CPU resources
    - Dedicated high speed LAN?
      - LAN deicated for intra cluster communication
        - Sometimes known as System Area Network (SAN)
      - Dedicated network for storage, Storage Area Network (SAN)
  - Examples of High Speed Interconnects
    - TCP/IP offload engines (TCP stack at the switch) (TOE)
    - Remote direct memory access (RDMA) - memory copy w/o CPU
    - Intel I/O acceleration (combines TOE with RDMA)
    - An example of this is Infiniband, a switch based point-to-point bidirectional serial link
      - Links processors, IO and storage
      - each link has one device
      - enable data movement via RDMA
      - 25Gbps/link
      - Aggregate links capable of 300 Gbps
    - Myricom's Myrinet
      - 10Gbps ethernet
      - Uses PCI express
    - IEEE 802.1 Data Center Bridging (DCB)
      - Set of standards to extend ethernet
      - lossless data center transport layer with priority-based flow control, congestion notification and bandwidth management.
- Disks
  - Shared storage access
    - Networked DFS
      - NFS, SMB, AFS, AFP, etc
      - Great for many applications
  - Concerns with availability, performance
  - Shared disk and cluster file systems
    - Shared disk - many systems have access to single drives
    - cluster file system
      - Client runs a file system accessing a shared disk at the block level
      - no client/server modes, or disconnected modes
      - All nodes are peers
    - Distributed lock manager
      - Ensures mutual exlusion for disk access
      - not needed for local file systems with shared disk
    - Clustered file system examples
      - IBM GPFS
      - MS Cluster shared volumes (CSV)
      - Oracle Cluster File System (OCFS)
      - Red Hat Global File System (GFS2)
  - Alternatives: shared nothing
    - No shared devices with each system having its own resources
    - No lock managers, but we need exclusive access to shared storage.
  - SAN - storage area network with separate network between nodes and storage arrays
    - AKA: DAS, SAN, NAS
- Failover
  - HA issues: detection, how long, how to restart, where do we move applications to
  - Hearbeat network - detect fault systems: distinguish system and network faults
  - Configuration models:
    - active/passive
      - Requests to active system, passive do nothing
    - Active/active
      - any node can handle requests
    - Active/passive N+M
      - M dedicated failover nodes for N active
  - Design options
    - Cold failover - app restart
    - Warm failover - restart last checkpoint
    - Hot failover - app stte sync'd across systems, spare immediately ready
  - Multidirectional failover - apps migrate to restart on available system
  - Cascading: if backup fails, app restarted on a surviving system
  - IP address Takeover (IPAT)
    - Ignore
    - Take over IP
    - Take over MAC
    - Listen on multiple addresses
  - Hardware support for HA
    - Hot pluggable componets
    - Redundant devices
    - Diagnostics
  - Fencing
    - Method of isolating a node from a cluster
      - disconnect IO, avoid byzantine, avoid fail-restart issues
    - Types:
      - Power (shut off)
      - SAN (disable fiber channel to node storage)
      - Disable access to global network device
      - Software (remove node from cluster)
  - Cluster software hierarchy
    - Top tier: cluster abstractions (Namenode/masters)
    - Middle: distributed operations (Paxos/Raft)
    - Bottom: OS and drivers
- High Performance Computing
  - Supercomputers are not typically created with commodity computers
  - Target complex, scientific applications
  - Many custom efforts (Linux + MPI + remote exec & monitoring)
  - MPI: Message passing interface
    - API for sending and receiving messages
    - Scalable IO, dynamic process management, synchronization, combining results
  - PVM: Parallel Virtual machine
    - Software to emulate general purpose heterogenous framework on interconnected computers
    - PVM presents a library to create tasks, use global task IDs, manage tasks and pass messages between tasks
  - Clustering for performance 
    - Beowulf  - clustering with linux
      - BProc: Beowulf distributed process
        - Start processes on other machines
        - global proces IDs
      - Network device drivers
      - File system
        - NFS root
        - Unsynchronized
        - Periodically synchronized via rsync
      - Program with PVM and MPI
      - Distributed shared member
      - Cluster monitor
      - Global ps, top, uptime
      - Process management
    - See Rocks cluster
    - Employed on over 1,300 clusters
    - Mass installation
    - Rolls: collection of packages for cluster
    - Microsoft HPC pack - clustering with windows and windows server
      - Systems management, networking, Jobs scheduler, Storage, failover support
      - Uses head node, broker nodes, compute nodes
- Batch Processing
  - Non-interactive processes
    - MapReduce, simulation tasks, graphics rendering
  - Single-queue work distribution: render farms
  - OpenPBS.org
    - portable batch system, developed by veridian for NASA
    - submit job scripts
    - list jobs
    - delete jobs
    - hold jobs
- Load Balancing
  - Load balancer handle load balancing, failover, and planned outage management
  - simplest method is to issue an http redirect if asked for a site
    - Trivial, successive requests, however customer sees changed URL
  - Use load balancing routers, more than packet forwarding
    - Assign one or more virtual addresses to a physical address
    - Pick machine with fewest TCP connections, factor in weights when selecting machines, round robin, etc.
  - Persistence
    - Send all requests from one user session to the same system.
  - DNS-based load-balancing
    - Round robin DNS -respond to DNS requests with a list of addresses instead of one
    - Order of the list permuted with each response
    - Geographic-based DNS response
      - Directs to servers in nearby regions
        - Ex: Amazon Route53

### Cryptographic Systems

Cryptography is not the same as security

Cryptography is good for:

- Authentication
- Integrity
- Nonrepudiation
  - Sender should not be able to falsely deny a message was sent
- Confidentiality

- Restricted Ciper
  - Secret algorithm to encrypt/decrypt. i.e. caesar cyphers
  - Not viable
- Symetric Algorithm
  - Known algorithm to everyone but must have key to encrypt/decrypt
  - Ex: AES, #DES, IDEA, RC5
  - Key length determines the number of possible keys
  - $$ 2^{\text{# bits}} = \text{Number of possible keys}$$
  - Brute force attach tries all keys
  - Both parties must know key beforehand and key distribution must be secret
  - For n users $$ \frac{n(n-1)}{2}$$ keys
  - Secure distribution is the biggest problem with symmetric crypto
  - Diffie-Hellman key exchange
    - First algorithm to use public/private keys
    - NOT public key encryption
    - Uses a one-way function based on difficulty of computing discrete logarithms
    - Allows us to negotiate a secret common key without fear of eavesdroppers
    - All arithmetic performed in a field of integers modulo some large number.
      - Private key for user $$X_i$$
      - Public key for user $$ Y = \alpha^{X_i} \text{ mod } p $$
      - $$K = \text{Bob's public key}^\text{Alice's Private key} \text{ mod } p $$
      - $$K' = \text{Alice's public key}^\text{Bob's Private key} \text{ mod } p $$
      - $$ K = Y_B^{X_A} \text{ mod } p $$
      - $$ K = ( \alpha^{X_b} \text{ mod } p)^{X_a} \text{ mod } p $$
      - $$ K = \alpha^{X_bX_a} \text{ mod } p $$
- Public Key Algorithm
  - RSA Public key Cryptography
  - Each user generates separate private and public keys
  - Algorithm based on difficulty of factoring large numbers (keys are pair of large approx. 300 digit number)
  - Unlike symmetric key, not every number is a valid key
  - share public keys, private key must be secret.
  - Hybrid cryptosystems
  - One user encrypts with other users public key, same user decrypts with their own private key
    - Session key: randomly generated key for one comm session
    - Use a public key algorithm to find session key
    - Use symmetric algorithm to ecnrypt data with session key
    - public key algorithms are rarely used to enrypt messages
      - Much slower and vulnerable to plaintext attacks
      - RSA is 55x slower to encrypt and 2000x slower to decrpyt
    - in a hybrid system, client picks a session key
      - Send session key encrypted with public key to user who decrpyts with their private key
- Message authentication
  - compute easy in one direction, difficult in another
  - i.e. $$pq = N$$ is easy, but $$p, q$$ given $$N$$ is difficult.
  - Validate:
    - Creator (signer or content)
    - Content not modified
  - Digital signatures: PKC
    - Encrypting a message with a private key is the same as signing it.
    - Encrypt msg with alice's private key, lookup public key from directory, check if we can decrypt
  - Not always what we want -what if we want to verify the content:
    - Use digests (crypto hash functions)
    - Output is a fixed-length bit string
    - One way, collision resistent, efficient.
    - SHA-2/3, MD5
  - Digital signatures and hash functions
    - Create a hash of the message
    - Encrypt message w/ private key and send it with the message
    - Recipient:
      - Decrypts the encrypted hash with public key
      - Computes the hash of the received message
      - Compares decrypted hash with message hash
      - If same, then the data has not been modified
    - Msg Auth vs Signatures
      - Message auth codes (MAC)
        - Hash of a message ecnrypted with a symmetric key where an intruder cannot replace hash
      - Digital signature
        - Hash of message encrypted w/ owner's private key
      - Provides **non-repudiation**
    - Covert AND authenticated messaging
      - Combine encryption with a digital signature
      - Use a session key
        - pick key to encrypt with symmetric algo
        - K with public key of each recipient
        - for signing, encrypt the hash with sender's private key

### Authentication and Authorization

- Authentication - ensure users are who they say they are
- Integrity - verify data not compromised
- Confidentiality -prevent unauthorized access
- Availability - ensure system is accessible

Multi-factor authentication

- Example ATM card + PIN code

Two password is not 2F authentication!

**PAP**: Password authentication protocol

- Give login an password to server which checks if they exist in a database
- PAP reusable password
  - Problem 1: open access to password file
    - Solution: store hash of the password in the file

**dicitonary Attacks**

- Tests lists of common passwords, dictionary words, names
- Add common substitutions, prefixes, and suffixes

Speed up dictionary attacks by using a table of pre-computed hashes.

- We can use **salt** to combat this by adding it to the password, it is not a secret value. Even by knowing salt you can find the password because you can't use precomputed hashes

- Problem 2 of PAP: network sniffing by observing users sessions
  - Fix with one-time passwords
    - Sequence, time, or challenge based
    - S/Key produces a one/time password
      - Authenticate alice for 100 logins, give list of logins to alice as authentication
      - i.e. alice gives "Alice", $$x_{100}$$
      - Host computes $$f(x_{100})$$ and compares is to value in DB
```
if x100 provided by alice = passwd("alice")
  replace x101 in db with x100 by alice
  return success
else:
  fail
```
      - Next time: alice presents $$x_{99}$$
    - Use CHAP: Challenge-Handshake Auth Protocol
    - Challenge is a **nonce** (a set of random bits)
    - Create a hash of the nonce and secret
    - Intruder does not have secret and cannot find the secret
    - MS-CHAP
  - Use encrypted communication channels
- SecureID from RSA
  - Proprietary device from RSA
  - 2FA based on shared-secret key (something user has)
    - Stored on card/chip
  - Shared personal ID (PIN), known by user
  - Look up users pin and seed associated with token
    - Uses time of day to find clock drift and shift pattern to check number generation
    - add or subtract offset to determine what clock chip on securID card believes is current time
  - Passcode is cryptographic hash of seed, PIN, and time
    - Server computes `f(seed, PIN, time)`
  - Server compares results with data sent via client.

Man in the Middle Attacks (MITM)

- Attacker poses as the server with keys
  - Must use covert communication channels, can't send keys over public channels
  - May use signed messages
  - Intruder cannot modify signed messages

**Combined Authentication and Key Exchange**

- 3rd party has access to all keys (trent)
  - trent validates all keys
- Symmetric enryption used from transmitting session keys
- Wide-mouth frog
  - Trent:
    - Look up key corresponding to Alice
    - Decrypts remainder of alice's messages
    - Sees bob as destination
    - Looks up bob's key
    - Creates new message w/ new timestamp, identifies source of session key (alice)
    - encrypts message for bob
    - send to bob
  - Bob:
    - Decrypts message with private key
    - Validates timestamp
    - Extraxts alice as sender
    - extracts session key, K

There is now a secure communication channel


**Kerberos**



    


