---
description: This page explains indexing services.
---

# Indexing

Since only the commitments and nullifiers of created and consumed resources are stored on a distributed ledger (see [#creation](../resources/#creation "mention") and [#consumption](../resources/#consumption "mention")) and not the [resource object ](https://specs.anoma.net/latest/arch/system/state/resource_machine/data_structures/resource/index.html)itself, a node wanting to know about current global blockchain state must

* (1) be online all the time,
* (2) observe all transactions that occur, and&#x20;
* (3) store all created and consumed resource objects on their hard drive.

For a normal user, this is not feasible. Moreover, the above assumes that all transactions are transparent and not [shielded or private](../page/information-flow-control.md), in which case a node would not be able to see them.

**Indexing** is the process of **tracking** created and consumed resources **and storing the information** in a database. This database can then be filtered with custom predicates by service users. Users wanting to know, for example, their current balance, after being offline for a couple of days can query an indexing service for resources they might have received.

Indexing service providers can commit to archiving:

* All visible state
* App-specific state (e.g., based on the [resource kind](../resources/#resource-kind))
* User-specific state (e.g., based on ownership or other resource properties)

Commercial indexing services are expected to charge fees. However, since becoming an indexer is permissionless and a simple service commitment within the Anoma protocol, normal peer-to-peer nodes can become indexers, too. Moreover, nodes in the network can also commit to caching resources relevant for their peers free of charge for a certain period of time, thus contributing to a decentralized and mutual service economy.

{% hint style="info" %}
**Current private devnet**\
A central Anoma node is providing indexing services and allows filtering resources by kind and owner.
{% endhint %}
