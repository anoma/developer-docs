# Transaction Object

```agda
type Transaction :=
  mkTransaction@{
    roots : List Nat;
    actions : List Action;
    delta : Delta;
    deltaProof : Nat;
  };
```

* **`actions`:** Contains
* **`roots`:** Computed for each consumed resource.
* **`delta`:** Computed for each consumed resource.
* **`deltaProof`:** Computed for each consumed resource.

For more details you can visit the [Anoma specs page](https://specs.anoma.net/latest/arch/system/state/resource_machine/index.html).
