---
layout: default
---

## Deployment

There are multiple paths that could theoretically be used to introduce CISA to
Bitcoin. It has been pointed out [here](https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2018-March/015838.html)
and [here](https://github.com/BlockstreamResearch/cross-input-aggregation/tree/master?tab=readme-ov-file#integration-into-the-bitcoin-protocol)
that a new Segwit version may be the safest option, but more research is welcome
and needed before a proposal can be formalized.

### Soft fork requirement and security considerations

Activation of CISA, no matter in which form, requires a soft fork since it adds
a new verification algorithm. This has serious security implications. Bitcoin
can only be as secure as the weakest of its verification algorithms and should
spending with aggregate signatures turn out to be vulnerable, this would also
put all funds that can be spent with Schnorr signatures at risk even if Schnorr
signatures themselves remain secure.

## Related Proposals

A few proposals for Bitcoin consensus changes make use of signature aggregation.
In the spirit of Taproot, which included multiple changes/BIPs that could have
been deployed separately, it is interesting to keep an eye on these proposals
and investigate if it may be interesting to integrate them into a wider proposal.

### Graftroot

Proposes the possibility to delegate spending of an output to a different
script, called a surrogate script. As these surrogate scripts can be chained,
the signatures for each of the delegations can be aggregated to keep on-chain
overhead minimal.

[Read the mailing list post](https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2018-February/015700.html)

### Entroot

Improvement of Graftroot, combining it with the idea of [G'root](https://lists.linuxfoundation.org/pipermail/bitcoin-dev/2018-July/016249.html).

[Read the mailing list post](https://gist.github.com/sipa/ca1502f8465d0d5032d9dd2465f32603)
