---
icon: database
description: This page introduces Anoma's state model.
---

# State Model

A unique feature of Anoma protocol is its state model, which organizes state in atomic units called of [resources](../resources/) that can be created and consumed in [transactions](../transactions/). This **resource model** generalizes the UTXO model in the sense that resources can have arbitrary state and predicates expressing the constraints under which they can be created and consumed.

<figure><img src="../../.gitbook/assets/state.png" alt=""><figcaption><p>A state space containing multiple resource objects: three coins, two leafs, a gear, and a potion.</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/resource-model.png" alt=""><figcaption><p>A state transition consuming a coin and a cogwheel and creating a coin. <br>Note that an <a href="../resources/#ephemeral-resources">ephemeral</a> gear resource must be created to balance the transaction and is not shown here.</p></figcaption></figure>

## Affordances

Anoma's unique state model enables[^1] the following affordances to developers and users

* Heterogeneous trust
  * Resources can live on different controllers (e.g., L1's, L2's, three friends in a LAN).
  * A transaction can consume a resource on controller A and create it on controller B, thus enabling fluent cross-chain transfers.
* Information flow control
  * Transactions can be sent transparent, shielded, or private just by setting a flag.
* Intent-level Composability
  * Intents (unbalanced transactions) can be composed and settled across different applications and chains

[^1]: https://ethresear.ch/t/rfc-draft-anoma-as-the-universal-intent-machine-for-ethereum/19109.
