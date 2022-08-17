---
description: References:https://lamport.azurewebsites.net/pubs/time-clocks.pdf
---

# Distributed System : Timing

Every computer system will have a notion of clock. This is needed for the Operating System can schedule various processes. This is also used to synchronize with time of day etc. In a single computer there is one master clock and all other clocks are derived from this. Let us look at how computer gets its time. The computer has a quartz oscillator and this drives the counter circuitry. This counter starts with an initial value and decrements by 1 for each oscillation and generates an interrupt when it reaches 0. The initial value is reloaded. The interrupt is caught by the OS and maintains a clock for example 60 interrupts per second etc

However in a distributed System each computer has its own clock and hence there is a need for synchronization. Now the question is why is a synchronization needed? This is mainly because clocks can drift. The drift is because in the computer systems the clocks are generated using crystal quartz resonators. This is a whole topic on its own and we will not get into too much details here. These provide a base clock and the operating system can schedule various events using these clocks. So in a distributed system if the clocks drift by few microseconds it creates a major problem. Say for example ordering of transactions can matter in split seconds. So for example look at the below diagram. Alice needs to transfer to Bob and then only Bob can transfer to charlie. If the order of transaction changes the Bob does not have sufficient balance to transfer to charlie.&#x20;

![](<.gitbook/assets/image (11).png>)

Hence the order in which the messages arrive matter in distributed systems and network latencies can help compound the problem.



In distributed systems the clocks can be synchronized using Physical clock or Logical clock.

**Physical Clock**\
****In this case the computers circuitry is used to derive the clock. When multiple computers are participating in distributed system then following needs to be taken care\
1\) Clock Drift: Computers see a widening gap in perceived time \
2\) Clock Skew : The difference between two clocks at a given point in time.



Computers use Coordinated Universal Time (UTC). UTC uses International atomic time and its introduced from 1 Jan 1972. This can be used to measure drift in distributed systems. The below figures show clock with respect to UTC

![](<.gitbook/assets/image (18).png>)

To compensate for these drifts we need should not move the time back. This can cause lot of confusions for future messages as ordering of messages becomes a mess. So instead we should look at adjusting the clock itself. If the clock is faster then we should slow it down and if its slower then we should make it fast. This can be adjusted using the computer circuitry.

![Skew wrt UTC](<.gitbook/assets/image (2).png>)

Now the question is who will provide the reference UTC clocks? One practical option is use a time server. Basically each computer in the system can query this time server and get the time. However this needs to account for network delays and also if the server fails then there is no way for computers to synchronize. This can be improved by using multiple time sources and then take an average from that. A Network Time protocol (NTP) provides hierarchical servers and this system can provide better results for clock drift. NTP uses a hierarchical, semi-layered system of time sources. Each level of this hierarchy is termed a _stratum_ and is assigned a number starting with zero for the reference clock at the top

**Stratum 0:** These are high-precision timekeeping devices such as [atomic clocks](https://en.wikipedia.org/wiki/Atomic\_clock), GPS or other [radio clocks](https://en.wikipedia.org/wiki/Radio\_clock). They generate a very accurate [pulse per second](https://en.wikipedia.org/wiki/Pulse\_per\_second) signal that triggers an [interrupt](https://en.wikipedia.org/wiki/Interrupt) and timestamp on a connected computer. Stratum 0 devices are also known as reference clocks. NTP servers cannot advertise themselves as stratum 0. A stratum field set to 0 in NTP packet indicates an unspecified stratum \[wikipedia Network Time Protocol].

**Stratum 1** These are computers whose [system time](https://en.wikipedia.org/wiki/System\_time) is synchronized to within a few microseconds of their attached stratum 0 devices. Stratum 1 servers may peer with other stratum 1 servers for [sanity check](https://en.wikipedia.org/wiki/Sanity\_check) and backup.They are also referred to as primary time servers.

**Stratum 2** These are computers that are synchronized over a network to stratum 1 servers. Often a stratum 2 computer queries several stratum 1 servers. Stratum 2 computers may also peer with other stratum 2 computers to provide more stable and robust time for all devices in the peer group.

**Stratum 3** These are computers that are synchronized to stratum 2 servers. They employ the same algorithms for peering and data sampling as stratum 2, and can themselves act as servers for stratum 4 computers, and so on.

![NTP Hierarchy](<.gitbook/assets/image (17).png>)

**Logical Clocks**\
In logical clocks instead of time of day the distributed systems agree on the events. So lets take 3 computers and if A sends a message m1 and B responds to that with m2 then A will understand the second message m2 but computer C would not make any sense because it received m1 after m2.

![](<.gitbook/assets/image (15).png>)

So Leslie Lamport introduced the concept of Happens Before. The paper introduced abstract point of view in which a clock is just a way of assigning a number to an event, where the number\
is thought of as the time at which the event occurred.  Lamport introduced an algorithm that forces re-sequencing of timestamps to ensure that the "happened before" relationship is maintained. Lamport's algorithm requires a monotonically increasing counter that has to be incremented when events take place. These events will have the clock value which is called Lamport timestamp. This is good for single event. However if we want to understand if two or more events are concurrent then using a vector clock will help solve the problem for a system where all nodes are trusted. However in a blockchain kind of scenario there is no guarantee that all nodes have seen all messages with timestamp < T. Need to use FIFO links and wait for message with timestamp ≥T from every node.&#x20;

**How blockchains solve this problem?**\
For a proof of work blockchain like bitcoin it is difficult to synchronize the respective physical clocks while the nodes are scattered around the world, and it is also possible that there are nodes that camouflage the clock. Resynchronizing the correct time between nodes by introducing Network Time Protocol (NTP) is a difficult technique. Incorporating the time stamp in the block, a mechanism very similar to the Lamport’s logic clock is prepared. As described in \[Bitcoin: A Peer-to-Peer Electronic Cash System, Satoshi Nakamoto], each node that performs a write operation to a block as a miner itself has a role as a time stamp server. However in bitcoin only the longest chain is legitimate, incorrect transactions are discarded after miner verification. Therefore, the order of blocks is uniquely determined with the lapse of time. As each time stamp increases, the previous time stamp is reinforced.

For Proof of stake protocol like Cardano, the same technicques as in bitcoin cannot be used. The Cardano blockchain is semi-synchronous and divided into epcohs. Each epoch is 5 days and inside each epoch the time is divided into slots. The nodes synchronize their physical clocks to NTP and use the logical epoch/slot to add blocks in blockchain. Since this is semi-synchronous, the nodes can determine which slot and epoch they are currently synced to. Also the slot leaders are decided at the beginning of the epoch but is confidential.

**Next Steps**\
****This completes the basics needed for understanding blockchain as distributed systems. In the next chapter we will look at the leader election challenge and how this is very peculiar problem for blockchains.&#x20;

{% embed url="https://youtu.be/fyM338wl9uw" %}

