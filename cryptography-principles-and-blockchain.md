# Cryptography Principles and Blockchain

\


In this chapter we will look at the cryptography basics and its use in blockchain. From the previous chapters you are aware that the blockchain is a trustless distributed system. There is no single party that ensures security. In such an environment it becomes critical that the communication between the nodes ensures the right identity. Two important concepts that are used in blockchain are

1\) Cryptographic Hash function

2\) Asymmetric Cryptography

\


**Cryptographic Hash function**

&#x20;A hash function is a mathematical function that converts a numerical input value into another compressed numerical value. The input to the hash function is of arbitrary length but output is always of fixed length. ![](<.gitbook/assets/Pasted Graphic.jpg>)

\


\


A hash function has following properties

1\) Computationally hard to reverse a hash function

2\) Given an input and its hash it should be hard to find different input with same value

3\) It should be hard to find two input functions of any length that map to same hash

\


The basic hash function takes 2 blocks of fixed size and then create a hash. Hashing algorithm takes this basic block and run in multiple rounds to get final value.&#x20;

\
![](<.gitbook/assets/Pasted Graphic 2.jpg>)

\


Since the first message becomes an input to second one and this continues until block N, this effect is known as avalanche effect

\


Message Digest (MD) and Secure Hash Functions (SHA) are popular hash functions. The main usage of Hash function in blockchain is data integrity. So along with the message the corresponding hash is also sent.  The recipient will take the received message re-hash it and then compare this computed hash with the received hash. If both match then the message can be trusted. This is not only useful for data integrity but also block integrity. If the hash of the block is saved along with the block then the blocks become immutable. This is because if someone tries to change the block then they will not be able to get the same hash as original one because of the properties of hash. The other advantage is that the hash always has fixed length.

The hashing is also used in proof of work. In PoW, miner uses partial hash inversions to prove that work was done. The average work that the miner needs to perform in order to find a valid message is exponential in the number of zero bits required in the hash value, while the other miners can verify the validity by executing a single hash function

\


**Asymmetric Cryptography**

Cryptography is needed in distributed systems mainly because of the trustless nature. Cryptography is a method of using advanced mathematical principles in storing and transmitting data in a particular form so that only those for whom it is intended can read and process it. Encryption is a key concept in cryptography — It is a process whereby a message is encoded in a format that cannot be read or understood by an eavesdropper. The technique is old and was first used by Caesar to encrypt his messages using Caesar cipher. A plain text from a user can be encrypted to a cipher text, then send through a communication channel and no eavesdropper can interfere with the plain text. When it reaches the receiver end, the cipher text is decrypted to the original plain text.

In symmetric cryptography a single key is used to encrypt and decrypt the message. The sender and receiver share this key and keep it a secret. However in blockchain there are multiple distributed nodes and hence it becomes impossible to use the symmetric cryptography.&#x20;

Public-key cryptography, or asymmetric cryptography, is a cryptographic system that uses pairs of [keys](https://en.wikipedia.org/wiki/Cryptographic\_key). Each pair consists of a public key (which may be known to others) and a private key (which may not be known by anyone except the owner). The generation of such key pairs depends on [cryptographic](https://en.wikipedia.org/wiki/Cryptographic) [algorithms](https://en.wikipedia.org/wiki/Algorithms) which are based on [mathematical](https://en.wikipedia.org/wiki/Mathematical) problems termed [one-way functions](https://en.wikipedia.org/wiki/One-way\_function).&#x20;

\
![](<.gitbook/assets/Pasted Graphic 4.png>)

The main idea is to use a large random number and then use a key generation algorithm to get the public and private key. Now lets look at how this is used.\


\
![](<.gitbook/assets/Pasted Graphic 5.png>)



\


The first scheme of things, message is encrypted using Alice’s public key and this can be decrypted only by Alice with her private key. This is known as public key encryption. Here the main concern is the secrecy of the message itself. Another scheme is Different-Hellman Key exchange.

\


\
![](<.gitbook/assets/Pasted Graphic 8.png>)

\


In this case a shared secret is created using public key of one participant and private key of another. This essentially creates a shared secret which can be decrypted. The Diffie–Hellman key exchange method allows two parties that have no prior knowledge of each other to jointly establish a [shared secret](https://en.wikipedia.org/wiki/Shared\_secret) key over an [insecure channel](https://en.wikipedia.org/wiki/Insecure\_channel).

The other important concept is digital signatures. So in the first case we encrypted the message but how to know that in fact the message was send by the correct sender? message is signed with the sender's private key and can be verified by anyone who has access to the sender's public key. This verification proves that the sender had access to the private key, and therefore is very likely to be the person associated with the public key. This also ensures that the message has not been tampered with, as a signature is mathematically bound to the message it originally was made from, and verification will fail for practically any other message, no matter how similar to the original message.&#x20;

\
![](<.gitbook/assets/Pasted Graphic 9.png>)

\


We talked about generation of keys using a large random number. But which one is suitable for this? The answer is elliptic curve multiplication.

Elliptic curve multiplication is a type of function that cryptographers call a "one-way" function: it is easy to do in one direction (multiplication) and impossible to do in the reverse direction ("division", or finding the discrete logarithm). The owner of the private key can easily create the public key and then share it with the world knowing that no one can reverse the function and calculate the private key from the public key.

Bitcoin uses a specific elliptic curve and set of mathematical constants, as defined in a standard called secp256k1, established by the National Institute of Standards and Technology (NIST). The first and most important step in generating keys is to find a secure source of entropy, or randomness. Creating a bitcoin key is essentially the same as "Pick a number between 1 and 2256." The exact method you use to pick that number does not matter as long as it is not predictable or repeatable.&#x20;

Starting with a private key in the form of a randomly generated number _k_, we multiply it by a predetermined point on the curve called the _generator point_ _G_ to produce another point somewhere else on the curve, which is the corresponding public key _K_. The generator point is specified as part of the secp256k1 standard

K = k \* G

where _k_ is the private key, _G_ is the generator point, and _K_ is the resulting public key, a point on the curve

A private key can be converted into a public key, but a public key cannot be converted back into a private key because the math only works one way.

\
![](<.gitbook/assets/Pasted Graphic 12.png>)

\


To visualize multiplication of a point with an integer, we will use the simpler elliptic curve over real numbers—remember, the math is the same. Our goal is to find the multiple _kG_ of the generator point _G_, which is the same as adding _G_ to itself, _k_ times in a row. In elliptic curves, adding a point to itself is the equivalent of drawing a tangent line on the point and finding where it intersects the curve again, then reflecting that point on the x-axis.

So this forms the basis of wallets and transactions in blockchains. The public private keys forms the basis for the address used in blockchains. The sender will use digital signature to help nodes to validate that the owner of the wallet. If we take Cardano’s example : if Alice wants to send Ada to Bob

1\) Alice creates a transaction with Bob’s public key based address and Ada to be transferred&#x20;

2\) Transaction hash is then created

3\) Alice then uses the private key to sign this hash

4\) This signature and the original transaction is sent.

5\) Bob verifies the received message by first verifying the signature to get the hash and calculates the hash of received message. This way Bob is able to identify the identity and also uncorrupted message from Alice

\


We will look into more on Wallets and transactions in next chapter. There are other cryptographic functions that we use in DeepchainAda viz, homomorphic encryption and decryption. We will look at this in detail when we talk about privacy based machine learning.

{% embed url="https://youtu.be/q66kJzjM3To" %}

References: Wikipedia and Mastering bitcoin

\


\


\


\


\


\


\


\
