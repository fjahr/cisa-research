---
layout: default
---

## FAQs

<details>
  <summary>How could CISA improve Bitcoin privacy?</summary><br>

  CISA by itself does not improve Bitcoin privacy.
  But the potential fee savings enabled by CISA could help make CoinJoins a much more widely adopted
  way of using Bitcoin on-chain. When using a CoinJoin-enabled wallet promises significantly
  lower fees, this should lead to wider adoptions by users. In turn, this could lead to more
  wallets adding CoinJoin integrations giving users more options matching their preferences.
  In a future where CISA is widely adopted, using CoinJoins may also be widely adopted and
  even be considered the default to interact with Bitcoin on-chain.

<br><br></details>

<!--
<details>
  <summary>Exactly how much cost-savings could CISA provide across different types of transactions?</summary><br>

  In general, there is no effect for transactions with one input because there is nothing to aggregate
  them with.

  Half aggregation:
  - Historical average transaction*: 7.6% WU (20.6% size)
  - CoinJoin (100 in/100 out):
  - Consolidation:
  - PayJoin: 3.7% WU

  Full aggregation
  - Historical average transaction*: 9.6% WU (26.1% size)
  - CoinJoin (100 in/100 out):
  - Consolidation:
  - PayJoin:

  * Historically the average transaction per https://transactionfee.info/ included 2.73 inputs and 2.8 outputs.

<br><br></details>
-->

<details>
  <summary>How meaningful might these cost savings be to different industry participants? Given historical precedent, could these cost savings make Taproot the default script type, as Native Segwit is today?</summary><br>

  The savings are very meaningful for all industry participants that have to deal with many
  on-chain transactions on a regular basis, such as merchants and exchanges. A good indication
  for this is the number of consolidation transactions that can be observed in a low-fee environment.
  The more consolidation pressure there is, the more fee savings the industry participants can get
  from using CISA with their consolidation transactions.
<br><br>
  It is very hard to project how quick adoption would progress, given that there is no full proposal
  at the moment. But the potential fee savings should motivate those participants of the network the
  most that historically often were the slowest to adopt new proposals: merchants and exchanges with volumes of
  on-chain transactions. On the side of user wallets, simply adding support for a new script type
  is not difficult, so this should not be a blocker. This means CISA could provide some additional
  momentum for adoption of a newer default script type.

<br><br></details>

<details>
  <summary>Could CISA, if adopted widely, reduce the effectiveness of chain analysis?</summary><br>

  CISA by itself does not reduce the effectiveness of chain analysis and might even be strengthening
  it. Introducing CISA with full aggregation to Bitcoin would mean that the common input heuristic
  could be assumed to be stronger for transactions that use full aggregation. Since full aggregation
  requires an interactive protocol which may not be widely at the time of a potential soft fork that
  adds CISA, the initial appearances of fully aggregated transactions are probably from a user that
  controls all inputs. This means that it is important to have the signature scheme ready as early as
  possible and give wallets time to adopt the scheme ideally even before the soft fork activates.
<br><br>
  However, the effectiveness of chain analysis can be reduced by a widened adoption of privacy
  protocols, such as CoinJoins and PayJoins. CISA has the potential to help the adoption of such 
  protocols because it makes transactions that use these techniques cheaper. This shifts the
  incentives for using these protocols from purely motivated by privacy to a combination of privacy
  and savings.

<br><br></details>

<details>
  <summary>Could CISA pave the way for cheaper transactions in a high-fee environment?</summary><br>

  Yes, all transactions with more than 1 input (assuming full-agg) would profit from fee savings
  when using CISA. The only requirement is that the spent UTXOs are already using an CISA-compatible
  output type. More inputs result in higher savings which leads to additional motivation to use
  collaborative protocols, such as CoinJoins, when spending funds. Once CISA has seen significant
  adoption this should lead to a lower overall fee level since the space savings that CISA bring
  also mean more transactions can end up in a block.

<br><br></details>

<details>
  <summary>Could CISA lower the minimum economically spendable account at a given fee rate?</summary><br>

  Yes, UTXOs that are spendable with CISA can be consolidated at lower costs than output types
  that can not use CISA.

<br><br></details>

<details>
  <summary>Could CISA help exchanges by, for example, saving them money while they consolidate small UTXOs?</summary><br>

  Yes, consolidation transactions are among the transaction types that would see the highest
  impact from signature aggregation because they consist of a lot of inputs, which means they
  also have a lot of signatures that would cause extra fees when compared to a CISA transaction.

<br><br></details>

<details>
  <summary>How does CISA compare to other aggregation strategies?</summary><br>

  CISA, i.e. signature aggregation, is related to but still very different from key aggregation
  that is more and more widely implemented post the Taproot deployment. Please refer to the
  <a href="https://cisaresearch.org/#signature-aggregation--key-aggregation-eg-musig-frost">specific section on this topic.</a>
  for more details.

<br><br></details>

<!--
<details>
  <summary>Are there any ways in which CISA could help upgrade or complement the Lightning Network, eCash, or existing CoinJoin mechanisms?</summary><br>

  - CoinJoin primary
  - Lightning no, musig use case
  - eCash no

<br><br></details>

<details>
  <summary>How might CISA protect the social and legal choice of Bitcoin users and businesses to CoinJoin, and the likelihood of more CoinJoin behavior, if users could state (if asked) that their decision was based on saving money, not simply seeking more privacy?</summary><br>

  TBD

<br><br></details>
-->

<details>
  <summary>How would Bitcoin Core need to change to integrate CISA? Are there any known current improvement proposals?</summary><br>

  There is currently no fully formulated proposal for CISA. The goal of this website is to show
  the different aspects that such a proposal could include and will have details on the proposal
  as soon as it is available.

<br><br></details>

<details>
  <summary>How would SegWit need to change to integrate CISA?</summary><br>

  The most likely way to deploy CISA would be with a softfork via a new SegWit version.

<br><br></details>

<details>
  <summary>Could CISA be packaged with (and might it be mutually beneficial with) any existing softfork convent proposals?</summary><br>

  Yes, CISA could be packaged with any of the softfork proposals being discussed currently. The
  deployment and signalling mechanism could work for all proposals as a whole or for each 
  proposal individually.

<br><br></details>

<details>
  <summary>Would CISA have any impact on output destruction versus creation? How would implementation relate to the witness discount?</summary><br>

  In Bitcoin transactions signatures are associated with inputs which destruct an output. Since
  signatures could be aggregated using CISA, the destruction of outputs would get cheaper while
  the creation would stay at the same price.
<br><br>
  The witness discount causes witness data, i.e. signatures, to cost less fees than the rest of
  the transaction. The discount thus softens the fee savings effect of CISA. This is also the
  reason that the space savings of CISA are different from the fee savings.

<br><br></details>

<details>
  <summary>How problematic would the interactive nature of full aggregate CISA be? What strategies might be adopted to ameliorate this?</summary><br>

  The interactive nature for full signature aggregation is similar to the one in key aggregation
  that is more and more widely implemented on top of Taproot. The solution to this is a secure
  signature scheme comparable to the role that MuSig plays for key aggregation. At the time of
  writing this, such a scheme still needs to be developed. After it has been developed it will
  need to be analyzed by cryptography researchers that, ideally, will be able to develop a security
  proof for it.
<br><br>
  Under the assumption that such a scheme can and will be developed, the interactive nature is
  not problematic taking aside the work that needs to go into developing the described scheme and
  the effort that will have to go into wallets and other apps that want to utilize full-agg CISA
  in an interactive protocol context.

<br><br></details>

<details>
  <summary>What would be the main downsides and risks of bringing CISA to Bitcoin?</summary><br>

  Any protocol upgrade comes with the risk of introducing bugs. While there are still many significant
  hurdles to clear for CISA to be considered a concrete proposal, it seems very likely that CISA will
  be proposal that is more limited in scope than most advanced script proposals and smaller in scope
  compared to SegWit and Taproot. Still, it is a changes that will require many hours of review from
  many people, including both cryptographers and software engineers.
<br><br>
  From a privacy perspective, there are two potential risks to name: The first is that users may be
  more eager to consolidate their funds since such transactions would be cheaper than they are today.
  However, consolidations need to be treated with caution as they can reveal common ownership. The
  second risk is related to this: In transactions generally the common input heuristic would probably
  considered to be stronger than today for any transaction with a fully aggregated signature.

<br><br></details>

<details>
  <summary>How would a CISA-powered for-profit service that could help people save money on fees, if they are willing to enter an interactive pool, work?</summary><br>

  Such a service would probably work similar to the for-profit CoinJoin services that already exist
  today. If privacy is not a feature that matters to users, a significant amount of features could
  be dropped by such a service, probably leading to further (but minor) additional fee savings and
  higher ease of use for its users.

<br><br></details>

<details>
  <summary>What other kinds of CISA-powered apps can be envisioned?</summary><br>

  A non-exhaustive list of ideas:

  <ul>
  <li>Any wallet can implement CISA to save on fees whenever it is consolidating outputs in one of its transactions</li>
  <li>Service providers like merchants and exchanges that frequently receive and consolidate UTXOs can use CISA to save a lot of fees when using CISA in their consolidation transactions</li>
  <li>PayJoin transactions can profit from fee savings in a similar matter as CoinJoins, even though their savings are smaller since their transactions tend to be a lot smaller with fewer inputs</li>
  <li>A batching server that batches PSBT signed with SINGLE|ANYONECANPAY sig hash could utilize half-aggregation to save fees for its users</li>
  </ul>

<br><br></details>
