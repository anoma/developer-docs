---
description: This page explains solving and counterparty discovery.
---

# Solving

Solving is the process of finding optimal composition of matching [intents](../transactions/intents.md), which is also commonly referred to as **counterparty discovery**. This involves finding matching intents in an **intent pool** (also referred to as the **intent gossip network**).

In mathematical terms, solving is a **constraint satisfaction problem** (see the [related ART report](https://zenodo.org/records/10019113)). Algorithm libraries, such as the [Google OR-Tools library](https://developers.google.com/optimization/mip) and the [SCIP](https://www.scipopt.org/) solver algorithm within, can be used to find solutions to these problems.

**Simple intents** can often be directly solved by the users themselves, i.e., by gossiping and listening to intents of connected peers and running standard solving algorithms on consumer hardware (e.g., your mobile phone). For example, swapping Pokémon or playing rock paper scissors with your friends does not require an external solver.

**Advanced intents** can involve multiple parties, oracles, and/or specific constraints. Accordingly, finding an optimal solution is much more difficult and will usually require specialized solvers.

We expect the solver landscape to consist of:

* **Specialized solvers** focussing on specific applications.
* **General-purpose solvers** being able to match intents across applications.

Furthermore, we expect standardized **intent formats** to emerge that solvers can interpret easily.

{% hint style="info" %}
**Current private devnet**\
A central Anoma node runs a brute-force solver. The solver tries to find matching intents in the intent pool by composing them. If the composed transaction is balanced, it gets executed.
{% endhint %}
