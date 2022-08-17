# Distributed Systems

As we discussed before, the base of a blockchain is a set of computers (nodes) and network. This is nothing but a classic distributed systems. Distributed systems have been around for many decades then why was blockchain not implemented earlier? The answer is the challenges in distributed system which made things more complicated for blockchains where untrusted parties had to communicate.

Before getting into these lets look at different distributed architecture\
1\) Server Client Architecture\
2\) Federated server architecture\
3\) Distributed peer to peer architecture

Figure 1 shows Server client architecture. There is one central server and all clients talk to this. This is very much used in many applications today. However this is not suitable in blockchain as we don't want one central server.&#x20;



![Classic Client Server](<.gitbook/assets/image (8).png>)

Figure 2 shows the Federated server architecture. In this case instead of one single server, there are multiple servers and they talk to each other. The other clients connect to one of the Federated server. These federated servers connect to one main server. This is not an ideal architecture for blockchain. If one server fails then all the clients connected to that server will get disconnected.&#x20;

![](<.gitbook/assets/image (16).png>)

Figure 3 is the peer to peer architecture. In this there is no central server and each node is independent and failure of one node does not cause any cascade effect.

![Peer to peer](<.gitbook/assets/image (10).png>)

The three main challenges in any distributed systems are \
1\) Network architecture\
2\) Node data liveliness\
3\) Timing

The below table shows the difficulty levels in each of the architecture for the above mentioned challenges

![](<.gitbook/assets/image (5).png>)

In the next section we will look into details of each of these challenges and why they are high for peer to peer.

{% embed url="https://youtu.be/Gvmh4lwc4j8" %}

