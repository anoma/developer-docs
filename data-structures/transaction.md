# Transaction

Transactions contain all the data required to conduct an Anoma state transition.

```agda
type Transaction :=
  mkTransaction@{
    roots : List Nat;
    actions : List Action;
    delta : Delta;
    deltaProof : Nat;
  };
```

* **`actions`:** A list of [actions](../learn/transactions/actions.md) constituting separate contexts for consumed and created resources.
* **`roots`:** A list of roots being required to prove that consumed resources have been created before.
* **`delta`:** The transaction delta indicating if a transaction is balanced or not.
* **`deltaProof`:** A proof that the transaction is balanced being required in the shielded case.

Further information can be found on [Anoma Resource Machine specs page](https://specs.anoma.net/latest/arch/system/state/resource_machine/index.html).

##
