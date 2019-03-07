# Distributed Systems Notes

## Gossip Protocol
* Push vs Pull protocols
    * Push are faster in the beginning O(log(n)) but pull faster on the second half O(log(log(n))), so push-pull protocols are good
* Packet loss and node failure is evenly distributed through the network, so it is very fault-tolerant, so with 50% packet loss, to achieve the same reliability as 0% packet loss you just need double the rounds (not that many)
* Resources:
    - [A Gossip-StyleFailureDetectionService](https://www.cs.cornell.edu/home/rvr/papers/GossipFD.pdf)
    - [SWIM: scalable weakly-consistent infection-style process group membership protocol](www.cs.cornell.edu/projects/Quicksilver/public_pdfs/SWIM.pdf)

## Group Membership Protocol
* _Failure detection_: once a node has found a failing peer, it communicates it to the rest of the cluster
    * _Prop Completeness:_ each failure is detected
    * _Props Accuracy:_ there is no mistaken detection
    * Both are impossible (if possible, then we could solve dist consensus as well), so we can guarantee completeness, but accuracy only partial/probablistic

    * Speed: Time to first detection of a failure
    * Scale: Equal load on each member
    * Scale: Network message load

    * Suspicion mechanism 

* _Disemination_: a method to communicate joins, leaves, failures to peer members
    * There are methods which piggy back on failure detection gossip style messages, so add dissemination messages on the ping/ack etc messages of the failure detection

* _Membership Reference_: data structure that keeps known members
    * All members table
    * Neightbors
    * Distributed Hashtables
    * Finger Tables

## Failure Detection
* Ack and Nacks
* _Centralized Heartbeating_: Con: leader might fail
* _Ring Heartbeating_: Con: Ring might get broken
* _All to all heartbeating_: Con: high load (might not be that much)
* _Gossip Style Failure Detection_: 
    * Nodes periodically gossip their membership list
    * Membership list keeps the time since last communication of all nodes

## Fault Tolerance
* _Byzantine fault tolerance_

## Routing
* _Finger tables_: are hash tables to find resources

## Clock Sync
* _NTP Protocol_
* _Cristians Algorithm_
* _Lamports Timestamps and Lamport Ordering_: Dont sync time, just respect causality (very important!)
* _Vector Clocks_: Imrpoves on Lamport's

## Global Snapshots
* _Definition_: state for each process and each channel, can be calculated in run time and collected in a distributed manner
* _Cut_: Time frontier at each process and each channel, all events that happen before this are "in the cut"
* _Consistent Cuts_: a cut `c` is consistent iff: for each pair of events `e` and `f`, such that event `e` is in the cut `C` and if `f -> e` (`f` happens before `e`) then event `f` is in the cut `C`
* _Chandy-Lamport Algorithm_: creates consistent cuts and records a glboal snapshot without interferance

## Properties > Safety and Liveness 
* _Liveness_: Guarantee that something good will eventually happen. EG: Completeness of failure detectors (every failure will eventually be detected).
* _Safety_: Guarantee that something bad will never happen. EG: There will never be a deadlock in a distributed transaction
* _Can be difficult to guarantee both_ in a distributed async system: eg:
    * A failure detector can't guarantee both Completeness (Liveness) and Accuracy (Safety)
    * A concensus protocol can't guarantee both Decisions (Liveness) and Correct Decisions (Safety)
* _Chandy-Lamport Algorithm_:
    * Used to calculate global snapshot
    * Used to detect global properties which are stable, Stable = once true, always true
        * Stable liveness example: once terminated always terminated
        * Stable non-safety example: once there is a deadlock, the deadlock will stay
        * All stable global properties can be detected using this algorithm, due to its causal correctness

## Multicasting
* _FIFO_:
    * _Multicasts whose send events are causally related_, must be received in the same causality-obeying order at all receivers
        * _Formally_: If `multicast(g, m) -> multicast(g, m')` then any correct process that delivers `m'` would already have delivered `m`
    * _Concurrent_: multicasts are not ensured to be delivered in the same order on different processes/channels
* _Total Ordering_: or Atomic Broadcast
    * _Ensures_ all receivers receive all multicast in the same order
        * Formally_: If a correct process `P` delivers message `m` before `m'` (independent of the senders), then any other correct process `P'` that delivers `m'` would already have delivered `m`
    * May have to delay delivery of some messages at sender
* _FIFO-total and Causal-total_: algorithms exist
* _Visrtual Synchrony_:
    * [https://www.youtube.com/watch?v=J076S7E33bo](https://www.youtube.com/watch?v=J076S7E33bo)

## Mutual Exclusion
* _Unique access_ to shared resources
* _Token based algorithms_: a message can pass tokens which allow access to a resource

## Election Algorithms
* _Electing a process for a special responsability_

## Concurrency Control
* _Transactions_
* _Serial Equivalence_
* _Pessimistic Concurrency_
* _Optimistic Concurrency Control_

## Replication Control
* _Two Phase Commit_

## Schedulling

## Distributed Shared Memory

## Other
* _Common_: Exponential backoffs for not overloading of any retry opperation (like sending Nacks)
* _P2P Systems_: Chord, Pastry, Kelips