---
description: The following describes Anoma's state model.
icon: database
---

# State Model

A unique feature of Anoma protocol is its state model. Anoma organizes state in a resource model.

<figure><img src="../../.gitbook/assets/state.png" alt="" width="178"><figcaption><p>Schematic depiction of state as resources.</p></figcaption></figure>

## Affordances

Anoma's unique state model enables[^1] the following affordances to developers and users

* Heterogeneous trust
  * Resources can live on different controllers (e.g., L1's, L2's, three friends in a LAN).
  * A transaction can consume a resource on controller A and create it on controller B.
* Information flow control
  * Transactions can be sent transparent, shielded, or private just by setting a flag
* Intent-level Composability
  * Intents (unbalanced transactions) can be composed and settled across different applications and chains

[^1]: https://ethresear.ch/t/rfc-draft-anoma-as-the-universal-intent-machine-for-ethereum/19109.
