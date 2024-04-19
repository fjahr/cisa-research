---
layout: default
---

### Welcome CISA friends!

This website aims to reflect the current state of research into applying
cross-input signature aggregation to Bitcoin and related protocols. It also
maintains a list of open tasks that contributors interested in the topic can
apply themselves.

## Overview

This page gives a very high-level overview of CISA before diving deeper into
the different topics in the rest of the pages on this website.

### Schnorr signature linearity property

Schnorr signatures have a linearity property that allow for simpler
aggregation of such signatures when compared to their ECDSA counterparts. Since the Taproot
softfork activation in 2021, Schnorr signatures
can be used in Bitcoin, which opens the door to proposals that could allow the
usage of aggregate signatures in the Bitcoin protocol in the future.

### History

The base for CISA was layed in BIP 340 (Schnorr signatures) and the activation
of the Taproot softfork in 2021. Originally, CISA was considered to be part of
the Taproot proposal, but this idea was abandoned in order to keep the complexity
of the proposal manageable.

### Signature Aggregation != Key Aggregation (e.g. MuSig, FROST)

A common source of confusion among bitcoiners new to this
topic is how CISA relates to another topic that has been under static
development throughout the past years: Multisignature with Key Aggregation
via protocols like MuSig or FROST.

With a protocol like MuSig, the involved keys are aggregated to a single
key and this single key signs a single message. Given `(n public keys, 1 message)`,
n parties identified by n separate public keys create a single signature
to sign the same message. The public keys pk_1, ..., pk_n can be aggregated
into a single public key agg_pk via an algorithm KeyAgg, and agg_pk is all
the verifier needs. The verification API is like this:
`bool VerifyMultisig(msg, agg_pk)`. In other words, the verifier does
not need to know who's behind agg_pk.

In a CISA context there are different keys
that sign a different message, a different input in the
transactions, but their signatures are still aggregated. Given
`(n pairs (public key, message)`:
n parties identified by n separate public keys, each having also have a
separate message to sign, create a single signature. Now look at the
verification API:
`bool VerifySigagg((pk_1, msg_1), ..., (pk_n, msg_n))`.
Here, the verifier really needs to know all the public keys, plus the one-to-one
association with messages.

Aside from a different aggregation result and the correspondingly different API
for verification,
process-wise the primary difference is that MuSig must be performed
during address creation, while CISA can be done at signing time or even later
depending on which flavor is used. These differences in the protocols also
lead to different security guarantees for each approach.

Still, what's true is that in practice the algorithms may look similar,
and in an upcoming (full) aggregation protocol, signing could look a lot like the
signing protocol in MuSig2. But we should really treat signature aggregation
and multisigs as separate concepts that provide different functionalities and
have different interfaces.

### Half-Agg vs. Full-Agg

There are two flavors of CISA whose trade-offs need to be evaluated and
contrasted to come to the best possible application for each of them. On a very
high level their key differences are: 1. Space savings: half-agg saves up to
almost half the space of the aggregated signatures, while the result of full-agg
is the same size as one single signature. 2. Interactivity: full-agg requires
interaction between the signers at signing time while half-agg does not.

Additionally, it should be noted that both techniques can be combined, meaning
the result signature of full-agg can be further aggregated with other signatures
using half-agg post signing time if when interactivity is prohibited.

### Applications

As part of the Bitcoin protocol, CISA can be be both used to aggregate signatures
of a single transaction, a set of transactions, as well as even a whole block.
Furthermore, CISA can also be used to aggregate signatures used in
off-chain/layer-2 protocols that rely on signatures, for example the gossip
layer of the Lightning Network.
