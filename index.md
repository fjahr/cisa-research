---
layout: default
---

### Welcome CISA friends!

This website aims to reflect the current state of research into applying
cross-input signature aggregation to Bitcoin and related protocols. It also
maintains a list of open tasks that contributors interested in the topic can
apply themselves to.

## Overview

This page gives a very high-level overview of CISA before diving deeper into
the different topics in the rest of the pages on this website.

### Schnorr linearity property

Schnorr signatures have a linearity property that allows for simpler
aggregation of such
signatures when compared to their ECDSA counterparts. Since the Taproot
softfork activation in 2021, Schnorr signatures
can be used in Bitcoin which opens the door to proposals that could allow
usage of aggregate signatures in the Bitcoin protocol in the future.

### History

The base for CISA was layed in BIP 340 (Schnorr signatures) and the activation
of the Taproot softfork in 2021. Originally, CISA was considered to be part of
the Taproot proposal but this idea was abandoned in order to keep the complexity
of the proposal manageable.

### Signature Aggregation != Key Aggregation (e.g. MuSig)

A common misconception or source of confusion among bitcoiners new to this
topic is how CISA relates to another topic that has been under static
development throughout the past years: MuSig. This [question was answered thoroughly on Bitcoin Stack Exchange](https://bitcoin.stackexchange.com/questions/106163/what-is-the-difference-between-key-aggregation-and-signature-aggregation),
here is just a short summary: With MuSig the keys are aggregated to a single
key and this single keys signs a single message, hence this process is more
concretely called Key Aggregation. In a CISA context there are different keys
that sign a different message each but their signatures are still aggregated.
Aside from a different result (1 key, 1 message, 1 signature vs. n keys, n
messages, 1 signature),
process-wise the primary difference is that MuSig must be performed already
during address creation while CISA can be done at signing time or even later
depending on which flavor is used.

### Half-Agg vs. Full-Agg

There are two flavors of CISA whose trade-offs need to be evaluated and
contrasted to come to the best possible application for each of them. On a very
high level their key differences are: 1. Space savings: half-agg saves up to
almost half the space of the aggregated signatures while the result of full-agg
is the same size as one single signature. 2. Interactivity: full-agg requires
interactivity between the signers at signing time while half-agg does not
require and interactivity.

Additionally, it should be noted that both techniques can be combined, meaning
the result signature of full-agg can be further aggregated with other signatures
using half-agg post signing time if when interactivity is prohibited.

### Applications

As part of the Bitcoin protocol CISA can be be both used to aggregate signatures
of a single transaction, a set of transactions as well as even a whole block.
Furthermore, CISA can also be used to aggregate signatures used in
off-chain/layer-2 protocols that rely on signatures, for example the gossip
layer of the Lightning Network.
