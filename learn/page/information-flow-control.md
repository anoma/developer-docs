# Information Flow Control

The Anoma team is currently working on three resource machine instances:

* The transparent resource machine (written in Elixir and Nock)
* The shielded Cairo resource machine (written in Rust and Cairo)
* The shielded RISC Zero resource machine (written in Rust for the RISC Zero zkVM)

In the **transparent case**, the transparent ARM computes the balance, re-evaluates the logic function and recomputes the commitments and nullifiers and checks for their presence in the commitment tree or nullifier set.

In the **shielded case,** the Cairo or RISC Zero ARM verifies balance, resource logic, and compliance proofs that the sender of the transaction has generated locally. This way, observers of the controller (e.g., a blockchain) don't see what resources are created and consumed — they only see commitment and nullifiers being added.

In the future, and with advances in cryptography, the fully **private case** may become feasible.

To learn more about the Anoma resource machine, visit the [Anoma specs page](https://specs.anoma.net/latest/arch/system/state/resource_machine/index.html)

{% hint style="info" %}
In the private devnet only the transparent resource machine instance can be used.
{% endhint %}
