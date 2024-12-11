# Actions

Actions are part of the transaction data structure and separate the transaction into distinct contexts.&#x20;

* Created and consumed resources can only see and related to each other if they are part of the same action.

```agda
type Action :=
  mkAction@{
    commitments : List Nat;
    nullifiers : List Nat;
    proofs : List Proof;
    appData : Nat;
  };
```

* **`commitments`:** Computed for each created resource.
* **`nullifiers`:** Computed for each consumed resource.
* **`proof`:** Logic and compliance proofs
* **`appData`:** A mapping containing arbitrary, application-specific data.
