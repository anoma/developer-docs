---
icon: laptop-binary
---

# Resource Machine

The **Anoma Resource Machine** (ARM) is a virtual machine being part of the Anoma Protocol and used to create, compose, and verify transactions. It is stateless and run by every node processing transactions.

## Overview

Before executing [transactions](../transactions/), the ARM conducts a number of checks.

1. **Balance check**\
   The number of consumed and created resources of the same kind must balance in a transaction.
2. **Resource Logic Checks**\
   For each resource in the transaction, the corresponding resource logic function must return `true`.
3. **Compliance Checks**\
   Ensures the resources lifecycle is not violated
   * Created resources
     * Commitments can not exist already.
   * Consumed resources
     * Commitment must exist already
     * Nullifier can not exist
   * Ephemeral resources (created and consumed)
     * existence checks are skipped



## Instances & Information Flow Control

The Anoma team is working on three resource machine instances:

* The transparent resource machine (written in Elixir and Nock)
* The shielded Cairo resource machine (written in Rust and Cairo)
* The shielded RISC Zero resource machine (written in Rust for the RISC Zero zkVM)

In the **transparent case**, the transparent ARM computes the balance, re-evaluates the logic function and recomputes the commitments and nullifiers and checks for their presence in the commitment tree or nullifier set.

In the **shielded case,** the Cairo or RISC Zero ARM verifies balance, resource logic, and compliance proofs that the sender of the transaction has generated locally. This way, observers of the controller (e.g., a blockchain) don't see what resources are created and consumed — they only see commitment and nullifiers being added.

To learn more about the Anoma resource machine, visit the latest [Anoma specs page](https://specs.anoma.net/latest/arch/system/state/resource_machine/index.html).
