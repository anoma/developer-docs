---
description: >-
  This page list the different information flow control cases and related ARM
  instances developed by the Anoma team.
---

# Information Flow Control

Transaction can be sent with different information flow control settings. Depending on the setting, the transaction is processed by a different resource machine instances.

In the **transparent case**, the executing node re-computes the transaction delta, re-evaluates the logic function, and re-derives the commitments and nullifiers and checks for their presence in the commitment tree or nullifier set. This requires the full information (i.e., the resource objects) to be visible to the executing node. Since the transaction is routed to a P2P network beforehand, the information is visible to all network participants.

In the **shielded case**, the shielded ARM verifies a transaction balance proof, resource logic proofs, and compliance proofs that the sender of the transaction has generated locally. This way, observers don't see which resources are created and consumed — they only see commitments and nullifiers being added. The parties generating the proofs and sending the transaction can share the information (i.e., the resource objects) voluntarily with other parties but don't have to.\
\
In the **private case**, data is known by no one independently and computed and stored in encrypted form using various forms of homomorphic encryption. Today, technological limitations still exist but with advances in cryptography being made, the fully private case can become feasible.

## Resource Machine Instances

The Anoma team is currently working on three resource machine instances:

* The transparent resource machine (written in Elixir and Nock)
* The shielded Cairo resource machine (written in Rust and Cairo)
* The shielded RISC Zero resource machine (written in Rust for the RISC Zero zkVM)

{% hint style="info" %}
**Current private devnet**\
In the current devnet, only the transparent resource machine instance is available.
{% endhint %}
