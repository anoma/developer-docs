---
description: The following compares Anoma's resource model with the account and UTXO model.
---

# Model Comparison

The advantages and unique affordances are best understood by comparison with other widely used state models, the account model and the UTXO model.

<table><thead><tr><th width="190"></th><th>Account Model</th><th>UTXO Model</th><th>Resource Model</th></tr></thead><tbody><tr><td>State Representation</td><td>Explict</td><td>Implicit</td><td>Implicit</td></tr><tr><td>State Scope</td><td>Global, unified across all accounts</td><td>Local, fragmented among UTXOs</td><td>Local, fragmented among resources</td></tr><tr><td>State Updates</td><td>Direct modification of global state</td><td>Consumption and creation of UTXOs</td><td>Consumption and creation of resources</td></tr><tr><td>Data Lookup</td><td>Direct lookup in the account.</td><td>Aggregation of UTXOs (usually coins of an owner).</td><td>Aggregation of unspent resources of specific kind.</td></tr><tr><td>Logic Predicates</td><td>Arbitrary logic per account.</td><td>One predicate for all resources.</td><td>Arbitrary predicates per resource.</td></tr><tr><td>Privacy</td><td>Transparent</td><td>Privacy </td><td>Information-flow control</td></tr><tr><td>Apps</td><td>General applications</td><td>Limited applications (e.g., payments)</td><td>General application &#x26; intents</td></tr><tr><td>App Distribution</td><td>Account deployment to an address to each chain.</td><td>Consumption &#x26; creation of resources.</td><td>Consumption &#x26; creation of resources.</td></tr><tr><td>Used by</td><td>Ethereum, Solana, Cosmos, Polkadot</td><td>Bitcoin, Zcash</td><td>Anoma</td></tr></tbody></table>

(\*It should be noted that protocols like Aztec and Cardano created an enhanced UTXO model enabling general applications.)



<table data-view="cards"><thead><tr><th></th><th></th><th></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td>A transaction updating the state in a smart contract in the account model.</td><td></td><td></td><td><a href="../../.gitbook/assets/account-model.png">account-model.png</a></td></tr><tr><td>A transaction consuming and creating UTXOs in the UTXO model.</td><td></td><td></td><td><a href="../../.gitbook/assets/utxo-model.png">utxo-model.png</a></td></tr><tr><td>A transaction consuming and creating different resources of different kind in the resource model.</td><td></td><td></td><td><a href="../../.gitbook/assets/resource-model.png">resource-model.png</a></td></tr></tbody></table>

