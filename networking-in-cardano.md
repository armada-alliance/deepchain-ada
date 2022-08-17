# Networking in Cardano

Networking in Cardano is being enabled for P2P. As you know blockchain need P2P for a truely decentralized environment. However in Cardano the P2P is being enabled in 1.33.0. I will update this chapter once P2P is enabled on mainnet

So how does Cardano handle node connectivity if its not P2P? It uses mechanism called topology updator. Relays for the first time need to connect to a known relay in network (usually provided by IOG). This is similar to a trusted setup we have during cryptograhic initializations. Once the relays are synced, if you operate a stakepool then you need to register the relay as part of your pool. This will help the relay get registered in the topology files. Once in the topology file the other realys can discover and connect. You need to run a updator script every hour so that topology has the relay entry. This aoids faulty relays to be part of topology

Currently the connections between nodes is unidirectional. This means you have seperate IN connections and OUT connections. P2P will enable bidirectional connections. I provide some more details in the video

Apart from this, there are mini-protocols to help implement the block transmission and connection to wallets. This is beyond the scope of this book. Usually these protocols are implemented by low level API.

This completes the Blockchain part. From next chapter we will look at Deeplearning

{% embed url="https://www.youtube.com/watch?index=14&list=PL-2d9cwY8dwSTSVsTzk3TKo38pascR88I&v=2uceGwdDL3c" %}

&#x20;&#x20;

