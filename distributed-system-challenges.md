# Distributed System Challenges

So to recap the main challenges in distributed system are\
1\) Network architecture\
2\) Node data liveliness\
3\) Timing\
One specific problem for blockchain is leader selection.

When we consider any blockchain we need to analyze how the blockchain tackles these challenges and the assumptions that are made.&#x20;

****\
**Network Architecture:**\
The architecture itself is usually peer to peer. As shown in previous page the centralized servers are not suitable for Blockchains as we dont want any central node to control the network. However some blockchains choose a federated approach so that until the blockchain grows the pioneers can protect the network. In this Federated approach there are several servers that are maintained by trusted entities and later these servers are removed from the network and make the network more and more peer to peer. Cardano was an example of this approach. By the time I started studying bitcoin and Eth, they were all peer to peer. Ofcourse you have something like a "mining pool" which can collate their compute power. We will look at this when we come to leader selection.\
&#x20;   Now the next challenge is to look at the behavior of the network. So in the peer to peer we need to understand the assumptions blockchain makes for network behavior. Network behavior is describing what is the expectation from the network. Do we assume that the network is fully reliable like a proprietary network or do we assume fully secure network. In general blockchains will work over internet and this generally means that blockchains have to assume that the network is unreliable and maybe unsecured also.

The blockchains need to take these assumptions and convert that to secure reliable communication by using concepts of secure protocol (for example https)  and for reliability have mechanisms that can support re-connection and re-transmission. We should also know the expectations of the network for these parameters. We will look at this in detail when we deep dive into Cardano.

**Node data Liveliness:**\
This deals with the challenge that the Node is always synced to the latest blocks in the ledger. This directly depends on the ability of the node to stay alive. As you know computers can fail, so in such a scenario how does the blockchain handle? What happens if one node fails? Does it affect the other nodes and reduce blockchain's capability? In most cases the blockchain protocol is capable of handling node failures because of peer to peer networking. The biggest challenge in blockchain is a node turning into an adversary. Now the node maybe working as honest in some scenarios and dishonest in other. So can the blockchain handle such a situation. This is usually referred to as Byzantine problem. \
Byzantine thought experiment: In this classic problem, the Byzantine army is separated into divisions, each commanded by a general. The generals communicate by messenger in order to devise a joint plan of action. Some generals may be traitors and may intentionally try to subvert the process so that the loyal generals cannot arrive at a unified plan. The goal of this problem is for all of the loyal generals to arrive at the same plan without the traitorous generals being able to cause them to adopt a bad plan. \


![Byzantine Problem](<.gitbook/assets/image (6).png>)

So as you can see above the blockchain nodes can suffer with this problem. Unfortunately we cannot trust any node. The practical solution to this was that if there are N dishonest nodes then we need total 3N+1 nodes in blockchain. So this means we cannot have more than 1/3 dishonest nodes. This was very limiting set. This can be achieved only in a federated architecture. However a blockchain should be able to tolerate upto 49% dishonest players. This was one of the reasons that held back blockchain development. The main thing that blockchain like bitcoin suggested was the Nakamoto consensus. The interesting thing in blockchain is that unlike distributed databases, in blockchain we can assume that the longest chain is selected as the main chain. However the leader who get selected for extending the chain has to perform a honesty check by using the Proof of Work or Proof of stake. In bitcoin PoW the node should have high compute capability to solve a cryptographic puzzle. So as long as the 51% of compute is not owned by a single entity the blockchain is guaranteed to have nakamoto consensus.  The other interesting thing with blockchain is that good behavior can be rewarded with the cryptocurrency. This encourages honest participation. Blockchains are prone to the Sybal attack which 51% attack. So if a single entity controls say the compute power in PoW or stake in PoS then they can cause this attack. So in Sybal attack the adversory has enough power to create new blocks faster than honest node and will always have his blocks extended as longest chain. In Nakamoto consensus we dont know who is honest and who is adversary and hence we go by the rule that longest chain is main chain.

In next section we will consider the Timing aspects in distributed computer system and how this relates to blockchain and its solutions.&#x20;

{% embed url="https://youtu.be/VG5kZ5n18jw" %}



\
&#x20;
