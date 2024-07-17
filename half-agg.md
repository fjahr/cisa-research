---
layout: default
---

## Half Signature Aggregation (half-agg)

The [BIP draft abstract](https://github.com/BlockstreamResearch/cross-input-aggregation/blob/master/half-aggregation.mediawiki#abstract)
provides a nice summary: "Half-aggregation is a non-interactive process for
aggregating a collection of signatures into a single aggregate signature. The
size of the resulting aggregate signature is approximately half of the combined
size of the original signatures."

#### Key benefits

- Space/Fee savings: Compared to a set of BIP 340 signatures which are 64 bytes
  each, a half-aggregate of these signatures would 32*n + 32 bytes. As part of
  a Bitcoin transaction the savings are 20.6% in terms of bytes and 7.6% in
  terms of weight units ([assuming the historically average transaction](https://github.com/BlockstreamResearch/cross-input-aggregation/blob/master/savings.org))
- Non-interactivity: The aggregation process does not require any cooperation
  between signers or with the aggregating party. This means that there is no
  requirement for signers to be online and signatures can be aggregated by
  other network participants, like broadcasting nodes or miners.
- Combination with full-agg: Signatures that are results of a full-agg process
  can be further aggregated with half-agg, leading to even higher savings

#### Status

A BIP draft has been written for the aggregation process of BIP 340 signatures.
Several possible applications have been proposed but also several open issues
remain. Several implementations have been written but all of them require
further review.

### Applications

The [motivation section](https://github.com/BlockstreamResearch/cross-input-aggregation/blob/master/half-aggregation.mediawiki#motivation)
in the half-agg BIP draft lays out applications in a bit more detail, below
is a summary of the key points.

#### Tx-wide half-agg

Aggregating all BIP 340 signatures in a single transactions into one half-agg
signature.

This is possible without a doubt and might even be the easiest to implement
protocol level use of half-agg signatures. However, many researchers at the same
time ask: why not use full-agg for this particular use case? Given that the
creation of a transaction among multiple participants always requires some sort
of cooperation, it is reasonable to assume that these protocols could handle
additional interactivity required for the aggregation of their signatures.

Aggregating signatures in transactions was also discussed as part of [Graftroot](https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2018-February/015700.html)
and could be part of other proposals that require verification of multiple
signatures.

#### Block-wide half-agg

Aggregating all BIP 340 signatures in a block into a single half-agg signature.
This seems like a good fit since half-agg does not require any interactivity
among the participants (which would be all transactions creators in the block).
However, there are still significant issues that need to be addressed for this use-case,
such as handling reorgs and adaptor signatures (see Open issues section).

#### Gossip protocol bandwidth savings in Layer-2 protocols

Particularly the [aggregegation of Lightning Channel Announcements](https://github.com/BlockstreamResearch/cross-input-aggregation/tree/master?tab=readme-ov-file#sigagg-case-study-ln-channel-announcements)
is discussed as [part of the BIP draft](https://github.com/BlockstreamResearch/cross-input-aggregation/blob/master/half-aggregation.mediawiki#motivation).
Since new channels are announced to the whole network via the gossip protocol
and these announcements are batched already, the signatures that are part of
these announcements could be aggregated to save bandwidth.

### Open issues

#### Adoptor Signatures/Scriptless Scripts and block-wide half-agg (assuming block-wide aggregation)

If you need a refresher: A good definition of Adaptor Signatures is [available
on the Bitcoin Optech website](https://bitcoinops.org/en/topics/adaptor-signatures/).

When used for block-wide signature aggregation in the naivest way, adoptor
signatures used in transactions in that block would not be usable anymore,
meaning their secret values could not be retrieved. This could potentially
break several protocols that are using adaptor signatures today, such as Atomic
Swaps and DLCs for example. A more detailed explanation is [available here](https://www.gijsvandam.nl/post/why-does-signature-half-aggregation-break-adaptor-signatures/).

There are certain scenarios where adaptor sigs are used that are not broken by
block-wide half-agg or that can still be feasible by extending the protocol with
script spend paths, [some of such examples are described here](https://github.com/BlockstreamResearch/cross-input-aggregation/blob/master/half-agg-and-adaptor-sigs.md).
However, there currently is no comprehensive list of protocols and their
specific implementation that are affected and not a clear plan on how to deal
with this issue.

#### Reorgs and block-wide half-agg (assuming block-wide aggregation)

In current bitcoin node implementations, transactions are typically removed
from the mempool once a block has been found that included this particular
transaction. This is ok because if this particular block is reorged out of the
best chain and the new chain doesn't contain any transactions from the first
block, these transactions can be recovered from the block and be put back into
the mempool.

This property is lost with block-wide half-agg. When the transactions are
removed from the mempool and only a block with one aggregate signature remains,
none of the included transactions can be recovered from this block. What follows
is that a different solution needs to be found for this issue, namely a
reorg-pool that keeps transactions for as long as the block is still considered
in danger to be re-orged. Otherwise, the original broadcaster of the
transactions would need to watch and rebroadcast in such a scenario. In theory
this could also be outsourced to a watchtower service.

This issue is also [described in a little more detail here](https://github.com/BlockstreamResearch/cross-input-aggregation/blob/master/README.md#half-aggregation-and-reorgs).

#### Mathematical security proof

While Schnorr signatures are provably secure just in the Random Oracle Model (ROM),
half-agg require both the ROM and the Algebraic Group Model (AGM). While this
probably not an issue in practice it would be great if AGM was not needed. For now,
this just means that this would not be as conservative of an update as Schnorr
signatures themselves.

### BIP

There is a draft BIP available for the half-aggregation process of BIP 340
signatures. There are currently no known BIPs or draft proposals for its
integration into the Bitcoin protocol.

[Draft: Half-aggregation of BIP 340 signatures](https://github.com/BlockstreamResearch/cross-input-aggregation/blob/master/half-aggregation.mediawiki)

### Code

There are currently three implementations of the half-aggregation
process available.

[hacspec/rust implementation (reference implementation of the BIP draft)](https://github.com/BlockstreamResearch/cross-input-aggregation/tree/master/hacspec-halfagg)

[C implementation included in secp256k1-zkp](https://github.com/BlockstreamResearch/secp256k1-zkp/pull/261)

[Implementation in Python based on BIP](https://github.com/fjahr/cisa-playground/blob/main/halfagg.py)

[PR for inclusion in secp256k1](https://github.com/bitcoin-core/secp256k1/pull/1566)
