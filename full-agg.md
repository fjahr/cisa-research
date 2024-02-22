---
layout: default
---

## Full Signature Aggregation

Full aggregation is an interactive process for aggregating a collection of
signatures into a single aggregate signature. The resulting aggregate signature
is of the same size as an unaggregated BIP 340 signature (constant).

#### Key benefits

- Space/Fee savings: A fully aggregated signature is of constant size, no matter
  how many signatures have been aggregated. The size is always 64 bytes, the
  same as a single BIP 340 signature. As part of a Bitcoin transaction the
  savings 26.1% in bytes and 9.6% in weight units ([assuming the historically average transaction](https://github.com/BlockstreamResearch/cross-input-aggregation/blob/master/savings.org)).
  When comparing these numbers with the half-agg savings, they don't seem to
  differ too much, but it should be noted that full-agg starts to shine the
  bigger the transaction gets. So, the savings get particularly interesting for
  large coinjoins or consolidation transactions.

### Applications

#### Block-wide full-agg

While theoretically possible, there seems to be consensus among researchers that
this is not a use-case worth pursuing due to the interactivity needed among
signers to produce a fully-aggregated signature. In any normally formed block
that includes transactions relayed over the P2P network, this level of
cooperation seems impossible to achieve.

#### Tx-wide full-agg

This is a feasible use-case for both transactions that produced by a single
signer as well as transactions with multiple signers that collaborate in an
interactive protocol. In the case of multiple signers, the protocol would need
to be extended to accommodate the steps to aggregate the signatures of the
transaction as well.

#### Gossip protocol bandwidth savings in Layer-2 protocols

Similar to the block-wide full-agg scenario the example use-case [Aggregation of Lightning Channel Announcements](https://github.com/BlockstreamResearch/cross-input-aggregation/tree/master?tab=readme-ov-file#sigagg-case-study-ln-channel-announcements)
for this scenario seems to not be a good fit for full-agg due to the extensive
interactivity needed the different signers.

### Open issues

#### Interactivity (assuming tx-wide aggregation)

As previously noted, the forming of a full-agg signature requires an interactive
process. Interactivity adds tremendous amounts of complexity to any protocol ([great talk on this subject](https://www.youtube.com/watch?v=uI15RKnyX_E)).
Existing protocols that want to integrate full-agg signature aggregation will
need to implement and handle this complexity in the future, including newly
introduced failure scenarios, privacy implications etc.

Additionally, to this date there has not been an multi-signature scheme
developed and suggested for the full-agg use case in Bitcoin transactions in
particular. [Bellare-Neven](https://cseweb.ucsd.edu/~mihir/papers/multisignatures.pdf)
can provide a starting point but more work has to be invested.

The following properties are desirable when it comes to this multi-signature
scheme:

- Provably secure
- Allow duplicate public keys
- Does not require proofs-of-possession
- Works with schemes like Taproot Tweaking and MuSig
- Two rounds like musig
- Batch verifiable

#### Common-input-ownership heuristic (assuming tx-wide aggregation)

This heuristic assumes that if a transaction that has more than one input, then
all those inputs are probably owned by the same entity. Due to the interactivity
constraints on full-agg signatures, this heuristic may be weighted heavier, and
this could lead to privacy implications for the users of such signatures in
their transactions.

### BIP

No BIP has been drafted publicly to this date.

### Code

No implementation as code is currently known.
