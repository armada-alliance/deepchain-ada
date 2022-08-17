---
description: This chapter will focus on Ouroboros Consensus
---

# Consensus Ouroboros

As we have studied before, blockchains need to solve the distributed system challenges like

1\) Node

2\) Network

3\) Time

4\) Consensus

We will first start with Consensus and time for Cardano. Cardano uses Proof of stake protocol and this is called Ouroboros. This helps in achieving consensus. Ouroboros comes in different flavors and these are

1\) Ouroboros Classic

2\) Ouroboros Praos

3\) Ouroboros Genesis

4\) Ouroboros Chronos

5\) Ouroboros Hydra

In this part we will focus on Ouroboros Classic and Praos. In the video below we will look at the Proof of stake protocol leader selection and usage of global time. We will look at how Verifiable Random Function (VRF) is used. This is part1 of the video. In part2 we will look at adversary and chain fork in these protocols





{% embed url="https://youtu.be/eIcl1EW9AFw" %}
Part1 ouroboros
{% endembed %}



The second part of the video talks about leader selection if there is a conflict. It may happen that at a given slot two parties are leaders then using VRF this is solved. This is called **slot battle**. The other kind of battle that happens if a party has propogation delays then it may mint same block number as another party. Then depending on the propogation time one of them is chosen. This is called **block height battle.**&#x20;

Apart from this there is chain selection that needs to happen and this also affects if a leader's block is added or not. This will be covered in next video

{% embed url="https://www.youtube.com/watch?v=mDI3Y5qn0rY" %}
