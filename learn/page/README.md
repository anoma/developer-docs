---
icon: laptop-binary
---

# Resource Machine

The **Anoma Resource Machine** (ARM) is a virtual machine being part of the Anoma Protocol and used to create, compose, and verify [transactions](../transactions/) that create and consume [resources](../resources/). The ARM is stateless and run by every node processing transactions.

## Overview

Before executing [transactions](../transactions/), the ARM conducts a number of checks.

1. **Balance Proof**\
   The number of consumed and created resources of the same kind must [balance](../transactions/) in a transaction.
2. **Resource Logic Proof**\
   For each resource in the transaction, the corresponding [resource logic function](../resources/#resource-logic) must is valid.
3. **Compliance Proof**\
   Ensures that the [resource lifecycle](../resources/#lifecyle) is not violated
   * Created resources
     * Commitments can not exist already.
   * Consumed resources
     * Commitment must exist already
     * Nullifier can not exist
   * Ephemeral resources (created and consumed)
     * existence checks are skipped

