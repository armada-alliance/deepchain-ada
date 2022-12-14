---
description: >-
  We will look at Wallet and how they help in generating payment keys and for
  blockchain like Cardano the staking keys also
---

# Wallets

We had talked about wallets during the introductory video on Blockchain. Wallets mainly provide the interfaces that is needed to interact with the Blockchain. The underlying asset of the blockchain for example Ada in Cardano or BTC in Bitcoin will be in the blockchain. Wallets only provide an interface to send/receive. So the coins are not really in the Wallet and Wallet mainly have the keys. We talked about the cryptographic principles in the previous section. We mainly talked about public and private key based cryptography. We saw that with this mechanism we can verify the ownership and tamper proof.

As we saw the keys are mainly generated from a seed. So wallets can be divided as deterministic wallet and nondeterministic wallet. In nondeterministic wallet the keys are newly generated every time and people have to maintain all the key pairs. This is great for privacy but painful to maintain. In case of deterministic wallet there is one seed and from this the wallet keys are generated. The advantage is that in case the wallet got corrupted or you wanted to change the wallet then using this seed you can create a new wallet.

However even maintaining this seed as backup is a challenge as it will be a long string containing a series of 1 and 0. The Bitcoin improvement proposal BIP39 introduced mnemonic based deterministic wallets. Also most wallets are hierarchical i.e. there are a bunch of keys that are generated from a master key.

First lets look at how the mnemonic is generated. Remember mnemonic words are generated automatically by the wallet using the standardized process defined in BIP-39. So the steps involved to generate this is as follows

1\) Generate 128 or 256 bit random number called entropy

2\) Take SHA256 hash. Get the checksum of length of entropy/32 from this hash

3\) Add this Checksum to the entropy

4\) Split this result into 11 bits each. For example if the entropy was 128bits then adding checksum, it will become 132bits. If we divide by 11 we will get 12 11bit numbers.&#x20;

5\) The BIP39 English word list maps words to 11 digit numbers. For example 00000000000 maps to word abandon

6\) For 128bit entropy get 12 mnemonic codes. For 256 it will be 24 words.

7\) We then can use **Password-Based Key Derivation Function** PBKDF2. PBKDF2 applies a [pseudorandom function](https://en.wikipedia.org/wiki/Pseudorandom\_function), such as [hash-based message authentication code](https://en.wikipedia.org/wiki/Hash-based\_message\_authentication\_code) (HMAC), to the input [password](https://en.wikipedia.org/wiki/Password) or [passphrase](https://en.wikipedia.org/wiki/Passphrase) along with a [salt](https://en.wikipedia.org/wiki/Salt\_\(cryptography\)) value and repeats the process many times to produce a _derived key_, which can then be used as a [cryptographic key](https://en.wikipedia.org/wiki/Key\_\(cryptography\)) in subsequent operations. So in our case the Mnemonic code words and a constant Salt is passed to PBKDF2 with 2048 rounds

8\) The output of the PBKDF2 will be the 512 bit seed. So as long as you remember the mnemonic words and the order, you can always generate this same key. Any slight change will not generate same key because of the cryptographic principles applied here

&#x20;From this seed, the first 256 bits can be taken to create a Master private key and generate Master public key using the same elliptic curve math.

From these keys the address are generated. Remember Cardano uses BIP32-Ed25519. Ed25519 is an elliptic curve standard for digital signatures. This is different from bitcoin which uses secp256k1

**How do we get the address from this seed?**

In Cardano from the seed, the wallet Public and Private Keys are generated. This key is then combined with

1\)  Purpose Pvt key: This is a value that is enoded as 1852H for shelly era

2\) Coin Type Pvt Key: This identifies the coin type and for Ada its 1815

3\) Account Private Key and Public key. Multiple accounts can be generated and the index of the account that is being used is added to get the address

4\) Role Pvt Key and Public Key. Right now there are mainly 3 roles, 0-> external chain, 1->internal chain and 2 is staking address.

5\) Address Pvt key and Public key. By default a wallet like Daedalus generates 20 address with indices from 0 to 19.

Once we get this address public key we can get the payment address. This can be used for sending and receiving transactions. In cardano you can stake and earn rewards. This staking keys are generated by setting the role as 2

This all can be done on commandline with cardano-wallet

_**cardano-wallet recovery-phrase generate > primary**_

_**cat primary | cardano-wallet key from-recovery-phrase Shelley > root.prv**_

_**cat root.prv**_\
_**| cardano-wallet key child 1852H/1815H/0H**_\
_**| tee account.prv**_\
_**| cardano-wallet key public --with-chain-code > account.pub**_

_**cat root.prv**_\
_**| cardano-wallet key child 1852H/1815H/0H/0/0**_\
_**| tee address.prv**_\
_**| cardano-wallet key public --with-chain-code > address.pub**_

To get Payment address

_**cat address.pub | cardano-address address payment**_\
_**--network-tag 1 > address.pay**_

For stake keys

_**cat root.prv**_\
_**| cardano-wallet key child 1852H/1815H/0H/2/0**_\
_**| tee staking.prv**_\
_**| cardano-wallet key public --with-chain-code > staking.pub**_



**Types of Wallets**

Until now we looked at the inner working of the wallet and interact with it using commandline. However this is not suitable for people who are not familiar with this infrastructure. Hence a wallet with a nice User interface is needed. In cardano you have mainly Daedalus and Yoroi. These help generate the required address for payment and staking without the need for any commadline

Another important aspect is to understand where the keys are stored when you use the wallet App. We have two options, hot wallet and cold wallet. Hot wallet is a wallet on mobile phone or desktop. The keys are on this device. Hence care should be taken that the device is not hacked or compromised. Cold wallets use specialized Hardware which only connects using USB and the keys are stored in this device. This provides enhanced security

As you can the Mnemonic words are very critical and so they should be written down and should not be stored on a device connected to internet. Anyone can get access to the wallet with these words.

In the next section we will talk about transaction and how wallets help in creating those transactions

{% embed url="https://youtu.be/XKs-skBubXs" %}

