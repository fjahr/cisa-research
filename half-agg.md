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

- Adoptor Signatures
    - https://bitcoinops.org/en/topics/adaptor-signatures/
    - https://github.com/BlockstreamResearch/cross-input-aggregation/blob/master/half-agg-and-adaptor-sigs.md

### BIP

[Draft: Half-aggregation of BIP 340 signatures](https://github.com/BlockstreamResearch/cross-input-aggregation/blob/master/half-aggregation.mediawiki)

### Code

[hacspec implementation (refernce alongside the BIP draft)](https://github.com/BlockstreamResearch/cross-input-aggregation/tree/master/hacspec-halfagg)

[Draft implementation on secp256k1-zkp](https://github.com/BlockstreamResearch/secp256k1-zkp/pull/261)

[Implementation in Python](https://github.com/fjahr/cisa-playground/blob/main/halfagg.py)
