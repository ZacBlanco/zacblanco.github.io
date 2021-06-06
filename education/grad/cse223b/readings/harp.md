

## Introduction

- replication technique used in the HARP filesystem
    - HARP (Highly Available Reliable Persistent) filesystem
- high probability, data in files is not lost
- modifications to files recorded at several nodes atomically
- primary-copy replication technique:
    - client writes to single server, doesn't respond to client until
      all copies to other nodes are successful
- records affects of modifications to a log, applied to FS in the background
- volatile log + UPS prevents sudden failures?
    - what about other hardware faults?
- implemented in the VFS interface


## Related work

- primary-copy replication technique
- witness schemes
- Harp ensures consistency unlike Coda/Locus
- HA-NFS technique for reliability:
    - write to shared disk which can be read by backup system

## Implementation

- meant to run on networked nodes
- typical processors with network that can deliver packets
- synchronized with < 100ms skew
- server nodes handle requests
- clients send requests
- servers should be equipped with a UPS


### Replication

- all requests make changes at primary first, then replicate
- modifications (writes) need a 2-phase protocol
    - first **inform** backups about the operations
    - return to client, _then_ let backups proceed to commit in background
    - changes not actually applied to the FS until committed to the log
- failure/failure recovery a failover protocol gets run: _view change_
    - essentially a new primary is selected, clients must be redirected to
    the new primary
- store n+1 copies to ensure availability
- organize system into groups so each node is the primary of one group,
  backup of another, and witness of a 3rd group
- normal case processing
    - primary log stored in volatile memory
    - distinguish between records which are committed/not committed with a _commit point (CP)_; the index of latest committed op.
    - return by committing a new CP then giving results
    - maintain a lower bound pointer (LB) to find out which commits have
      actually been persisted to disk
    - maintain global lower bound, GLB across all primary+backups
- upon failure, log is a "redo" log

### View changes

- correctness: operations must appear in order
- reliability effects of committed operations must survive all single failures
- availability: must provide service whenever any two group members are up
- timeliness: failovers must be achieved in a timely manner

- views number to determine which view the group has.
- designated witness will act as a backup where a primary or backup is missing
- witness takes part in processing file operations when promoted
    - witness does not have copy of FS, so many not apply committed ops
    - never discards entries from log
- witness promotion --> receive all log records not yet on disk of
  primary and backups
- bring other node up to date by reading witness log (as long as there
  was no disk failure)
- state of new view reflects all committed operations from previous
  views
- every committed op is at two servers
- view promoted witness lasts a long time -- can prune unneeded entries

- node starting the view change is the _coordinator_
    - coordinator communicates with other members to form a view view
    - once all nodes agree, move to phase 2
        - write new view number to disk and informs all nodes of the new
          view
    - on system crash, some volatile memory stays in order to recover
      lost FS state

- client switching:
    - ip multicast
    - insert changes to nfs client code, switch to new node when view changes
