---
layout: default
---

## Half Signature Aggregation

- size half of normal sigs
    - https://github.com/BlockstreamResearch/cross-input-aggregation/blob/master/savings.org
    - alone: 20.6% bytes but only 7.6% weight units
    - average transaction
- non-interactive

### Applications

- Block-wide signature aggregation
- Tx-wide signature aggregation
    - 
- Gossip protocol bandwidth savings in Layer-2 protocols
  - [Aggregegation of Lightning Channel Announcements](https://github.com/BlockstreamResearch/cross-input-aggregation/tree/master?tab=readme-ov-file#sigagg-case-study-ln-channel-announcements)

### Open issues

#### Adoptor Signatures/Scriptless Scripts and block-wide half-agg

If you need a refresher: A good definition of Adaptor Signatures is [available on the Bitcoin Optech website](https://bitcoinops.org/en/topics/adaptor-signatures/).

When used for block-wide signature aggregation in the most naive way, adoptor signatures used in transactions in that block would not be usable anymore, meaning their secret values could not be retrieved. This could potentially break several protocols that are using adaptor signatures today, such as Atomic Swaps and DLCs for example. A more detailed explanation is [available here](https://www.gijsvandam.nl/post/why-does-signature-half-aggregation-break-adaptor-signatures/).

There are certain scenarios where adaptor sigs are used that are not broken by block-wide half-agg or that can still be feasible by extending the protocol with script spend paths, [some of such examples are described here](https://github.com/BlockstreamResearch/cross-input-aggregation/blob/master/half-agg-and-adaptor-sigs.md). However, there currently is no comprehensive list of protocols and their specific implementation that are affected and also not a clear plan on how to deal with this issue.

#### Reorgs and block-wide half-agg

In current bitcoin node implementations, transactions are typically removed from the mempool once a block has been found that includedd this particular transaction. This is ok because if this particular block is reorged out of the best chain and the new chain doesn't contain any transactions from the first block, these transactions can be recovered from the block and be put back into the mempool.

This property is lost with block-wide half-agg. When the transactions are removed from the mempool and only a block with one aggregate signature remains, none of the included transactions can be recovered from this block. What follows is that a different solution needs to be found for this issue, namely a reorg-pool that keeps transactions for as long as the block is still considered in danger to be re-orged. Otherwise the original broadcaster of the transactions would need to watch and rebroadcast in such a scenario. Potentially this could also be outsourced to a watchtower serverice.

This issue is also [described in a little more detail here](https://github.com/BlockstreamResearch/cross-input-aggregation/blob/master/README.md#half-aggregation-and-reorgs).

### BIP

There is a draft BIP available for the half-aggregation process of BIP 340 signatures. There are currently no BIPs proposed for it's integration into the Bitcoin protocol.

[Draft: Half-aggregation of BIP 340 signatures](https://github.com/BlockstreamResearch/cross-input-aggregation/blob/master/half-aggregation.mediawiki)

### Code

There are currently three implementations available of the half-aggregation process.

[hacspec implementation (reference implementation of the BIP draft)](https://github.com/BlockstreamResearch/cross-input-aggregation/tree/master/hacspec-halfagg)

[Draft implementation on secp256k1-zkp](https://github.com/BlockstreamResearch/secp256k1-zkp/pull/261)

[Implementation in Python based on BIP](https://github.com/fjahr/cisa-playground/blob/main/halfagg.py)
