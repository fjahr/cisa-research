---
layout: default
---

## Welcome CISA friends!

This website aims to reflect the current state of research into applying
cross-input signature aggregation to Bitcoin and related protocols. It also
maintains a list of open tasks that contributors interested in the topic can
apply themselves to.

## Overview of Cross-Input Signature Aggregation

This page gives a very high-level overview of CISA before diving deeper into
the different topics in the rest of the pages on this website.

### Schnorr linearity property

Schnorr signatures have a linearity property that allows aggregation of these
signatures. Since the Taproot softfork activation in 2021, Schnorr signatures
can be used in Bitcoin which opens the door to aggregate signatures of such
transactions.

### Signature Aggregation != Key Aggregation

A common misconception or source of confusion among bitcoiners new to this
topic is how CISA relates to another topic that has been under static
development throughout the past years: MuSig. This [questions was answered thoroughly on Bitcoin Stack Exchange](https://bitcoin.stackexchange.com/questions/106163/what-is-the-difference-between-key-aggregation-and-signature-aggregation),
here is just a short summary: MuSig is not just aggregating signatures but
also keys, hence this process is correctly called Key Aggregation. CISA
referrs to only the aggregation of signatures. Aside from a different result,
process-wise the primary difference is that MuSig must be performed already
during address creation while CISA can be done at signing time or even later
depending on which flavor is used.

### History

The base for CISA was layed in BIP 340 (Schnorr signatures) and the activation
of the Taproot softfork in 2021. Originally, CISA was considered to be part of
the Taproot proposal but this idea was abandoned in order to keep the complexity
of the proposal manageable.

### Half-Agg vs. Full-Agg

There are two flavors of CISA whose trade-offs need to be evaluated and
contrasted to come to the best possible application for each of them. On a very
high level their key differences are: 1. space savings: half-agg saves up to
roughly half the space of the aggregated signatures while the result of full-agg
is the same size as one single signature. 2. Interactivity: full-agg requires
interactivity between the signers at signing time while half-agg does not
require and interactivity.

Additionally, it should be noted that both techniques can be combined, meaning
the result signature of full-agg can be further aggregated with other signatures
using half-agg post signing time if when interactivity is prohibited.

### Application

As part of the Bitcoin protocol CISA can be be both used to aggregate signatures
of a single transaction, a set of transactions as well as even a whole block.
Furthermore, CISA can also be used to aggregate signatures used in
off-chain/layer-2 protocols that rely on signatures, for example the gossip
layer of the Lightning Network.
