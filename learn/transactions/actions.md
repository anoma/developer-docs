---
description: This page describes the structure of the action object in detail.
---

# Actions

Actions are part of the [transaction data structure](transaction-object.md) and separate the transaction into distinct contexts. Created and consumed resources can only see and relate to each other if they are part of the same action.

```agda
type Action :=
  mkAction@{
    commitments : List Nat;
    nullifiers : List Nat;
    proofs : List Proof;
    appData : Nat;
  };
```

* **`commitments`:** Computed for each created resource (see [#creation](../resources/#creation "mention")).
* **`nullifiers`:** Computed for each consumed resource (see [#consumption](../resources/#consumption "mention")).
* **`proof`:** A list of logic and compliance proofs for each resource referred in the `commitments` and `nullifiers` fields.
* **`appData`:** A mapping containing arbitrary, application-specific data, encoded as a natural number.
