---
description: This page introduces the Anoma Resource Machine (ARM) and its functionalities.
icon: laptop-binary
---

# Resource Machine

The **Anoma Resource Machine** (ARM) is a virtual machine and part of the Anoma protocol. It is used to create, compose, and verify [transactions](../transactions/) that create and consume [resources](../resources/). The ARM is stateless and run by every node processing transactions.

## Transaction Checks&#x20;

Before executing a [transaction](../transactions/), the ARM ensures it is **balanced & valid.** This is done by checking various proofs:

1. **Transaction Delta (Balance) Proof**\
   The number of consumed and created resources of the same [kind](../resources/kind.md) must [balance](../transactions/) in a transaction. In other words: the transaction delta must have a value of 0.
2. **Resource Logic Proof**\
   For each resource in the transaction, the corresponding [resource logic function](../resources/logic.md) must be valid. This means that all resource logic functions must return `true`.
3. **Compliance Proof**\
   Ensures that the [resource lifecycle](../resources/lifecycle.md) is not violated:
   * [Created ](../resources/lifecycle.md#creation)resources
     * Commitments can not exist already
   * [Consumed](../resources/lifecycle.md#consumption) resources
     * Commitments must exist already
     * Nullifiers can not exist
   * [Ephemeral](../resources/lifecycle.md#ephemeral-resources) resources (that can be created or consumed)
     * Existence checks are skipped

The nature of this proof-checking depends on the [information flow control](information-flow-control.md) setting of the transaction.
