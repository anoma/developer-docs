---
description: This page compares Anoma's resource model with the account and UTXO model.
---

# Model Comparison

The advantages of the **resource model** are best understood by comparison with other widely used state models: the **account model** and the **UTXO model**.

<table><thead><tr><th width="162"></th><th>Resource Model</th><th>UTXO Model</th><th>Account Model</th></tr></thead><tbody><tr><td><strong>State Representation</strong></td><td><a data-footnote-ref href="#user-content-fn-1">Implicit</a></td><td><a data-footnote-ref href="#user-content-fn-2">Implicit</a></td><td><a data-footnote-ref href="#user-content-fn-3">Explict</a></td></tr><tr><td><strong>State Scope</strong></td><td>Local, fragmented among resources</td><td>Local, fragmented among UTXOs</td><td>Global, unified across all accounts</td></tr><tr><td><strong>State Updates</strong></td><td>Consumption &#x26; creation of resources</td><td>Consumption &#x26; creation of UTXOs</td><td>Direct modification of global state</td></tr><tr><td><strong>State Lookup</strong></td><td>Aggregation of  resources (e.g., by kind or owner)</td><td>Aggregation of UTXOs (e.g., by owner)</td><td>Direct lookup in the account</td></tr><tr><td><strong>Logic Predicates</strong></td><td>Arbitrary logic per resource</td><td>The same logic for each UTXO</td><td>Arbitrary logic per account</td></tr><tr><td><strong>Apps</strong></td><td>General application &#x26; intents</td><td>Limited applications (e.g., payments)</td><td>General applications</td></tr><tr><td><strong>App Distribution</strong></td><td>Consumption &#x26; creation of resources</td><td>Consumption &#x26; creation of UTXOs</td><td>Account deployment to an address to each chain</td></tr><tr><td><strong>Used by</strong></td><td>Anoma</td><td>Bitcoin, Zcash</td><td>Ethereum, Solana, Cosmos, Polkadot</td></tr></tbody></table>

<table data-view="cards"><thead><tr><th></th><th></th><th></th><th data-hidden data-card-cover data-type="files"></th></tr></thead><tbody><tr><td>A transaction consuming and creating different resources of different kind in the resource model.</td><td></td><td></td><td><a href="../../.gitbook/assets/resource-model.png">resource-model.png</a></td></tr><tr><td>A transaction consuming and creating UTXOs in the UTXO model.</td><td></td><td></td><td><a href="../../.gitbook/assets/utxo-model.png">utxo-model.png</a></td></tr><tr><td>A transaction updating the state in a smart contract in the account model.</td><td></td><td></td><td><a href="../../.gitbook/assets/account-model.png">account-model.png</a></td></tr></tbody></table>



[^1]: Example:\
    A balance of a certain resource kind is not explicit, but implicitly represented by the set of all resources of this kind owned by a certain owner.

[^2]: Example:\
    A balance is not explicit, but implicitly represented by the set of all UTXOs of this owner.

[^3]: Example:

    A balance in a balance mapping in a smart contract.
