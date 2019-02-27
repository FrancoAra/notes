# Distributed Systems Notes

## Gossip Protocol
* Push vs Pull protocols
    * Push are faster in the beginning O(log(n)) but pull faster on the second half O(log(log(n))), so push-pull protocols are good
* Packet loss and node failure is evenly distributed through the network, so it is very fault-tolerant, so with 50% packet loss, to achieve the same reliability as 0% packet loss you just need double the rounds (not that many)
* Resources:
    - [A Gossip-StyleFailureDetectionService](https://www.cs.cornell.edu/home/rvr/papers/GossipFD.pdf)
    - [SWIM: scalable weakly-consistent infection-style process group membership protocol](www.cs.cornell.edu/projects/Quicksilver/public_pdfs/SWIM.pdf)

## Group Membership Protocol
* _Failure detection_, once a node has found a failing peer, it communicates it to the rest of the cluster
    * _Prop Completeness:_ each failure is detected
    * _Props Accuracy:_ there is no mistaken detection
    * Both are impossible (if possible, then we could solve dist consensus as well), so we can guarantee completeness, but accuracy only partial/probablistic

    * Speed: Time to first detection of a failure
    * Scale: Equal load on each member
    * Scale: Network message load
* _Disemination_, a method to communicate joins, leaves, failures to peer members

## Failure Detection
* Ack and Nacks
* _Centralized Heartbeating_: Con: leader might fail
* _Ring Heartbeating_: Con: Ring might get broken
* _All to all heartbeating_: Con: high load (might not be that much)
* _Gossip Style Failure Detection_: 
    * Nodes periodically gossip their membership list
    * Membership list keeps the time since last communication of all nodes

## Clock Sync
* _NTP Protocol_

* _Common_: Exponential backoffs for not overloading of any retry opperation (like sending Nacks)