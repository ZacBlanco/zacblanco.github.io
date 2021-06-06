## Introduction

- monolithic filesystems
    - filesystem admin requires a large computer and ability to add more
    physical disks
    - manually moved/replicate when components fill
    - even with raid, cannot scale well
- frangipani
    - _scalable_ FS
    - assume secure communication
- features
    - all users get consistent view
    - more servers added easily without changing other servers
    - sys admins can make full and consistent backup of fs without bringing it down
    - fs tolerates and recovers from failures without operator intervention
- layered on top of Petal dist. disk system.
    - Petal allows writing of blocks, but not files
    - sparse 64-bit addr. space
    - may replicate+snapshot data
- frangipani adds minor modules on the client side, and lock server to the petal
servers


## System Structure

- distributed petal servers
    - contains multiple disks
    - frangipani lock server on each petal server
- frangipani server and fs switch on each client machine where user
  programs operate
- last-access time is only approximate
- frangipani servers do not communicate directly with one another
    - communicate only with petal and lock service

- security
    - all OSes must be trusted
    - not all nodes need to be entirely restricted, only frangipani
      servers which communicate with Petal must be secure

## Disk Layout

- first region of frangipani stored shared configuration and
  housekeeping information.
- up to one 1TiB of virtual space for housekeeping
    - far more than necessary
- 2nd region of disk is logs
    - 1TB for logs, partitioned across 256 equal spaces
        - limits to 256 servers, could be changed
- 3rd region is bitmaps, describing free/non-free regions
    - 3TB of bitmap
- 4th region holds inode metadata
    - 512-byte inode (same as block size, reduces contention)
    - 1TB of space for inodes
- 5th region holds small data blocks (4KiB)
    - 1st 16 blocks (64KiB) stored in small blocks
    - files > 64KiB then stored in one large block
    - 1TiB of address space for the rest of the large blocks
- limit of about 16M large files
- max file size is 64KiB + 1TiB

## Logging and Recovery

- write-ahead redo logging of metadata to simplify failure recovery
- each frangipani server has its own metadata log
- log bounded in size, up to 128KiB
- number of tolerated failures of frangipani depends on petal having
  metadata stored
- when failure is detected, runs recovery, which attempts to restore
  server by replaying the log from the failed server
- attach monotonically increasing log sequence numbers to
- lock servers ensure updates are serialized (important for consistency
  guarantees on failure)
- frangipani does not provide high-level guarantees to users
- locking protocol ensures updates are serialized
  - write lock on dirty data only changes owners after data is written
    to petal
- upon recovery, makes sure to still have access to the locks

## Synchronization, Cache-Coherence

- use read+write locks
- flush dirty data to disk when write lock is released/downgraded
- locks across segments of the disk space
- on-disk data structure to not span segments.
- operations which require several updates by ordering and acquiring in 2 phases


## Lock Service

- multiple reader/single writer locks (RWLock)
- locks stick - retained by client until another needs
- clients use leases on locks (lock with expiration)
- upon network error, raise a flag and error until unmount+remount operation(!)
- multiple impls
    - in-mem single server, slow recovery, fast performance
    - logging lock operations, better recovery times, generally worse performance
    - mutually cooperating lock servers, fault tolerant+clerk module
        - locks organized in tables
        - uses heartbeats to other servers to determine availability
- upon crash, all locks held by crashed server cannot be released until
  recovery ops are performed
- potential problem for issues with a margin time where network error
  occurs for some small amount of time between updating lock and writing to disk

## Adding and Removing Servers

- tell server its petal disk and lock service location, then it joins
- removing server --> shut it off

## Backup

- petal snapshots for full dumps of a frangipani FS
- includes all logs, so simple to restore
- snapshot recovery time is same as power-off recovery time

## Performance

- 4.3GB disks with 6MB/s writes and 11ms latency, 4% cpu usage
- metadata operations on frangipani much slower than direct disk
  counterpart, 2-5x
- frangipani has lower CPU utilization
- adding more machines has little effect on latency
- performance for reads/writes limited by switch/network
- suffers from lock contention performance issues
-