---
description: In this chapter we will focus on how transactions are constructed
---

# Transactions in Cardano

In the previous chapter we looked at how the wallets help in creating addresses. The wallets now have to support transactions like payment, registration of pool or after the Shelly fork in Cardano even the minting of tokens.

**Assets in ledger**

The first thing we need to understand is the ledger model. In other words, how are the assets (Ada) stored in the ledger. There are two main models for this. One is Unspent Transaction Outputs (UTxO) and other one is global accounting model.

In UTxO model, the assets are held as unspent transaction outputs. This is similar to cash and coins. For example if you want to make a purchase worth $90 and you have $100 note then you will give that and get back $10. So if we take this as UTxO model then you have one UTxO say worth 100 and after you spend it you get two UTxO, one worth 90 which is added to shopkeeper's wallet and another UTxO worth 10 is returned back to your wallet

In Account based model, transaction updates look similar to say a bank account. If you have $100 in the account then after spending $90 the account is directly updated to $10. This type is suitable for computation but the global state has to be known so that we dont have the double spend problem.

**UTxO**

Cardano uses Chimeric ledger. This means it can support both Account based and UTXO based transactions. When you stake in cardano stakepool and receive the rewards for staking then the reward amount is stored in Account based model. This helps in saving space and also if the rewards are very small then it prevents the problem of UTXO dust. Hence when you want to spend your rewards you first need to do a withdraw so that these get converted to UTXO.&#x20;

Now lets understand this UTXO transaction model

![](<.gitbook/assets/image (12).png>)

Suppose Alice's wallet has 3 UTXO, one worth 2Ada, another 3Ada and one more worth 100Ada. Alice wants to transfer 10Ada to Bob. In this case the wallet needs to select the UTxO worth 100Ada and once the transaction is successful then we will mainly have one UTxO worth 90Ada put back into Alice's wallet and UTxO 10Ada  in Bob's wallet. So now Alice again has 3 UTxO one worth 2Ada, another 3Ada and finally the new 90Ada. Note that we cannot split UTxO. They get spent and removed once they are part of successful transaction.



![](<.gitbook/assets/image (13).png>)

**EUTxO**

The EUTxO is the new enhanced UTXO to support smart contracts. In this case instead of just using the digital signature, we have a state called Datum and script. The input redeemer will check if the transaction can be redeemed. We will talk about this in detail when we start with smart contracts

In summary, each transaction has a set of UTxO to be spent and create new UTxO to handle the transfer and change. Every transaction has to pay a transaction fee to cover the cost of validating the transaction.

**Transactions in Cardano**

We now look at how the transactions are constructed. All these are represented using Concise Binary Object Representation (CBOR). CBOR is a data format whose design goals include the possibility of extremely small code size, fairly small message size, and extensibility without the need for version negotiation. Its similar to JSON. Like JSON it allows the transmission of data objects that contain [name–value pairs](https://en.wikipedia.org/wiki/Attribute%E2%80%93value\_pair), but in a more concise manner. This increases processing and transfer speeds at the cost of [human readability](https://en.wikipedia.org/wiki/Human-readable\_medium)

The basic steps are

1\) Find all the UTxO and choose the ones that will be suitable for the transaction (UTXO IN in fig below)

2\) Build the transaction using payment address, destination address and timeout. This timeout is needed incase the transaction does not complete within certain slots. If transactions are not processed before timeout then the transactions become invalid (UTXO OUT , Invalid Slot Num)

3\) Based on this calculate the fee. In Cardano fee is calculated with fee = minFee + 44\*NumberOfBytesInTransaction

4\) Calculate the change output. This is the value left after the transfer amount and fee (UTXO End address)

5\) Using these 4 parameters build the transaction as shown in figure below

<img src=".gitbook/assets/image (7).png" alt="" data-size="original">

Now if you see there are additional fields in the transaction. This is because after Shelly, we are able put metadata and create policies for NFT (non fungible tokens). This is one of the reasons why Cardano was able to support NFT even before smart contracts. So along with UTXO we can specify a policy ID to mint along with token name and number of tokens. The script hash is also added and a metadata which can be an IPFS location for the image. If we dont associate image the metadata will be NULL

6\) Finally we sign the transaction. This scheme was explained in the cryptography principles chapter.

![](<.gitbook/assets/image (1).png>)

The signed transaction will include the token public key (while minting tokens) and transaction hash signed with private key. Finally to pay the transaction fees in case of minting token or if making an Ada payment then payment key signature is added as shown in figure.

This was a very quick view on how wallets assist in creating transactions and submitting them to chain.&#x20;

We talked about UTXO dust. So what is UTXO dust? As we can see if we have multiple UTXOs then how do we choose which UTXO to be used. For example if we have in a wallet say one UTXO worth 2 Ada, another worth 1Ada, a third one worth 6 Ada. If we want to transfer 3 Ada which ones should we choose? Care should be taken that there are not too many small value UTxO as they may cause transaction fees to increase. If we include multiple UTXO then the size in bytes of transaction increases and hence fees. Hence if a wallet has lot of small value UTxO then its called accumulating dust. The wallets handle how to avoid this and we will not look at those details here. However if you are writing a contract that directly creates these transactions then care should be taken about UTxO dust.&#x20;

{% embed url="https://youtu.be/S57dfNZtYfo" %}

