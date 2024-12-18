---
description: This page describes the structure of the action object in detail.
---

# Actions

Actions are part of the [transaction data structure](transaction-object.md) and separate the transaction into distinct contexts. Resource logics of created and consumed resources can only see and relate to other resources if they are part of the same action. This **context separation** is important to compose transactions with each other without causing interferences.

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
* **`proofs`:** Contains a logic and compliance proof for each resource referred in the `commitments` and `nullifiers` fields.
* **`appData`:** A map (encoded as a natural number) containing arbitrary, application-specific data.
