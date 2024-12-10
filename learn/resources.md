# Resources

Resources are atomic units of application state and logic. They have

* a quantity, label, and specific kind
* data (e.g, owner, diameter, etc.)

| 5 🐚 `{diameter : 1.4}` | 2 🍏 `{fruitiness : 8 / 10}` | 1 💌 `{text : "I ❤️ u"}` |
| :---------------------: | :--------------------------: | :----------------------: |
|    `5 Shell` resource   |    `2 GreenApple` resource   |   `1 Message` Resource   |

*   a lifecycle with three stages,

    ```mermaid
    flowchart LR
      Non-existent --> Created --> Consumed
    ```
* a logic function enforcing predicates (that check data, e.g., in itself, the transaction or other resources)

<details>

<summary>The <code>Resource</code> object in detail</summary>

```haskell
type Resource :=
  mkResource@{
    logic : LogicRef;
    label : LabelRef;
    value : ValueRef;
    quantity : Quantity;
    ephemeral : Bool;
    nonce : Nonce;
    randSeed : RandSeed;
    nullifierKeyCommitment : NullifierKeyCommitment;
  };
```

* **`logic`:** A boolean-valued function enforcing predicates required to create and consume the resource.
* **`label`:** Arbitrary data describing the resource and determining its kind (e.g., the name or symbol).
* **`value`:** Arbitrary data associated with the resource (e.g., the owner).
* **`quantity`:** The number of units that this resource describes.
* **`ephemeral`** A boolean expressing whether this resource is ephemeral or not, i.e., exists only during a transaction.
* **`nonce`:** A number ensuring the uniqueness of the resource.
* **`randSeed:`** A number to derive (pseudo)-randomness from.
* **`nullifierKeyCommitment`** A commitment to a secret nullfier key.

\*Types named `*Ref` are binding references to objects in BLOB storage.

</details>

&#x20;

### Creation

To create a resource, its _commitment_ must be computed by hashing the resource object. $$\texttt{commitment} := h_\texttt{cm}(\texttt{resource})$$&#x20;

$$
\texttt{kind} := h_\texttt{kind}(\texttt{logic},\,\texttt{label})
$$

After execution, the commitment is added to a Merkle tree.



### **Consumption**

To consume a resource its _nullifier_ must be computed by hashing the resource object and a secret called the _nullifier key_.

$$
\texttt{nullifier} := h_\texttt{nf}(\texttt{resource},\,\texttt{nullifierKey})
$$

After execution, the nullifier is added into a nullifier set.

### Kind

The _kind_ of a resource determines its fungibility and is computed as the hash of its _logic_ and _label_.

$$
\texttt{kind} := h_\texttt{kind}(\texttt{logic},\,\texttt{label})
$$

The kind is used to check if transactions are balanced.
