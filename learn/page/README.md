---
icon: laptop-binary
description: This page introduces the Anoma Resource Machine (ARM) and its functionalities.
---

# Resource Machine

The **Anoma Resource Machine** (ARM) is a virtual machine being part of the Anoma Protocol and used to create, compose, and verify [transactions](../transactions/) that create and consume [resources](../resources/). The ARM is stateless and run by every node processing transactions.

## Transaction Checks&#x20;

Before executing [transactions](../transactions/), the ARM conducts a number of checks. This is done by checking a number of proofs.

1. **Transaction Delta (Balance) Proof**\
   The number of consumed and created resources of the same kind must [balance](../transactions/) in a transaction. In other words: The transaction delta must be 0.
2. **Resource Logic Proof**\
   For each resource in the transaction, the corresponding [resource logic function](../resources/#resource-logic) must be valid. In other words: All resource logic functions must return `true`.
3. **Compliance Proof**\
   Ensures that the [resource lifecycle](../resources/#lifecyle) is not violated
   * Created resources
     * Commitments can not exist already.
   * Consumed resources
     * Commitment must exist already
     * Nullifier can not exist
   * Ephemeral resources (created and consumed)
     * existence checks are skipped

The way how the proof-checking is conducted differs depending on the [information-flow control](information-flow-control.md) setting of the transaction.
