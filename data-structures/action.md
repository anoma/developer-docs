# Action

```agda
type Action :=
  mkAction@{
    commitments : List Nat;
    nullifiers : List Nat;
    proofs : List Proof;
    appData : Nat;
  };
```

* **`commitments`:** Computed for each created resource (see [#creation](../learn/resources/#creation "mention")).
* **`nullifiers`:** Computed for each consumed resource (see [#consumption](../learn/resources/#consumption "mention")).
* **`proofs`:** Contains a logic and compliance proof for each resource referred in the `commitments` and `nullifiers` fields.
* **`appData`:** A map (encoded as a natural number) containing arbitrary, application-specific data.
