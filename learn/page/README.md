---
icon: laptop-binary
---

# Resource Machine

The **Anoma Resource Machine** (ARM) is a virtual machine being part of the Anoma Protocol and checks if transactions are adhering to its rules and are therefore valid.



It is used to create, compose, and verify transactions. It is stateless and run by every node that processes transactions.



## Overview

Before executing [transactions](../transactions/ "mention"), the ARM conducts a number of checks.

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
* The shielded resource machine (written in Rust and Cairo)
* The RISC Zero resource machine (written in Rust for the RISC Zero zkVM)

In the transparent case, only&#x20;







To learn more about the Anoma resource machine, visit the latest [Anoma specs page](https://specs.anoma.net/latest/arch/system/state/resource_machine/index.html).
