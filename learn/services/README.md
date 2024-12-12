---
description: >-
  This page introduces services that nodes running the Anoma protocol can
  provide each to each other and which applications and users can make use of.
icon: gear-complex
---

# Services

P2P nodes running the Anoma protocol can provide different services to each other. Elementary services include provisioning of&#x20;

*   **Storage**

    Storing a particular piece of data for a certain time period and responding promptly to data-availability requests.
* **Bandwidth**\
  Gossiping and routing packages to connected peers.
* **Compute**\
  Performing computations and providing proofs-of-correctness upon request.&#x20;
* **Ordering**\
  Ordering of transactions on request and maintaining safety (i.e., no double-spends)

More advanced services can result from the composition of elementary services, such as

* [**Indexing**](indexing.md) and custom filtering of resources (e.g., by kind and owner)
* [**Solving**](solving.md) of [intents](../transactions/intents.md) (i.e., finding the optimal composition of matching, unbalanced transactions)
* **Validation** duties (in Proof-of-Stake protocols)

Nodes providing these services are taking the role of **service providers** and will likely expect compensation from service users. Since every node in the network (i.e., your mobile phone) can participate in providing services to connected nodes permissionlessly, this enables a decentralized and mutal service economy.

{% hint style="info" %}
**Current private devnet**\
Basic indexing and solving services are provided by a central Anoma node. In the future, these services will be provided by different P2P nodes participating in the Anoma network.
{% endhint %}
