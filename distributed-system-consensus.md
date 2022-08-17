---
description: This chapter we will discuss about consensus.
---

# Distributed system: Consensus

The idea of consensus is that the distributed systems will agree on taking the decision needed for the task. In a client server architecture its quite simple because the server is always taking the decision. In case of say a federated system with multiple servers it becomes critical that there is a consensus among the servers involved. There are many consensus algorithms for distributed systems and it all depends on the type of network you want to build and the fault tolerance levels you plan for the network. Broadly speaking there are two types of fault assumption, Byzantine and non-Byzantine. We have talked about this previously. The table below summarizes the consensus types

![Choice of consensus protocols](<.gitbook/assets/image (3).png>)

So the simplest is round-robin and Paxos or RAFT are the types of consensus that are used in a distributed system but with limited number of servers. In this case the number of nodes are fixed and are trusted. These can be used in a trusted distributed system.

The Byzantine fault tolerance can tolerate only 33% of dishonest actors. Blockchains posed two main challenges for distributed consensus \
1\) Parties can be dishonest \
2\) Parties can join/Leave the network \
Hence we needed a more robust consensus algorithm which would be BFT but also support with atleast 51% honest actors. This 51% consensus is also known as nakamoto consensus because the PoW algorithm introduced by bitcoin supported this.

**Proof of Work**&#x20;

So the whole idea in a distributed consensus is that the leader is elected and all honest nodes agree with this election. Think about this like a timer. Say each node has a timer and when the timer expires they declare themselves as leaders and then publish that to the peers. Now the challenge here is that its very easy to hack the hw/sw to count the timer fast enough and keep declaring themselves as leaders and many peers could contest that. Hence there needed a mechanism where a node has to do some work which takes significant resource and then publishes the result which other nodes can verify. The good thing with this approach is that all computations are node specific and there is no other exchange of information during the consensus. This is where the concept of hashing comes handy. We will discuss this in detail when we talk about cryptography in the next chapter. Hashing means using some function or algorithm to map object data to some representative integer value. In case of blockchain we use a set of transactions. Now if only a set of transaction is used then its very easy to crack it. Hence in proof of work and also in proof of stake a "nonce" ( number used once) is added to set of transactions. This helps because hash is a one-way function: it cannot be used to obtain the original data, only to check that the data that generated the hash matches the original data. Also since the number of nodes in the network is dynamic and remember that a block needs to be produced at regular interval, a difficulty policy is added. If number of nodes are high then difficulty level is high and if nodes decrease then difficulty is lowered so that the interval is maintained. For bitcoin its 10mins

**Proof of stake**&#x20;

Another popular blockchain consensus protocol is proof of stake. Instead of using the computing power the proof of stake uses the concept of staking. In this case the amount of stake that node has will determine its chances of creating a block. In proof of stake there is additional challenge, there can be multiple nodes with similar stake and also nodes should have a saturation limit so that they don’t control maximum stake such that for every block interval the chances of them getting chosen is high. In case of say Cardano which uses ouroboros PoS, the protocol sets ideal number of nodes to be 500 and max stake to be 62M ada. This ensures fair distribution of block creation. Now again among these nodes how do you select the leader? Ouroboros processes transaction blocks by dividing chains into epochs, which are further divided into time slots. A slot leader is elected for each time slot and is responsible for adding a block to the chain. This leader selection uses again the cryptographic technique called VRF verifiable random function. Each of the node registered in the network. At every slot the node verifies a lottery happens and the node checks if its value is below the lottery value and then mints the block if it is elected leader. Even though the slot lottery numbers are randomly generated they are also deterministic when combined with epoch nonce and VRF key. We will cover this in detail when we talk about Cardano

There are many variations of Proof of stake and also other proof of history etc. In general the PoS uses far less compute resource and hence energy efficient.

So using these consensus protocol ensures that leader election is fair and secure in the dynamic environment of blockchain. Next chapter we will cover cryptography details used in blockchains for consensus as well as wallets.

you can find history of PoW here [https://armada-alliance.com/blogs/history-of-pow](https://armada-alliance.com/blogs/history-of-pow)&#x20;

{% embed url="https://youtu.be/RbDVIHC-SQA" %}



