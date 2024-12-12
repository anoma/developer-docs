---
description: This page describes the structure of the resource object in detail.
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Resource Object

The resource object is a data structure that contains multiple fields.

```agda
type Resource :=
  mkResource@{
    logic : Logic;
    label : Nat;
    value : Nat;
    quantity : Nat;
    nullifierKeyCommitment : Nat;
    nonce : Nat;
    randSeed : Nat;
    ephemeral : Bool;
  };
```

* **`logic`**\
  A boolean-valued function enforcing predicates required to create and consume the resource.
* **`label`**\
  Arbitrary data describing the resource and determining its kind (e.g., the name or symbol).
* **`value`**\
  Arbitrary data associated with the resource (e.g., the owner).
* **`quantity`**\
  The number of units that this resource describes.
* **`nullifierKeyCommitment`**\
  A commitment to a secret nullfier key required to shield the resources. This way, observers cannot link the creation and consumption of resources.
* **`ephemeral`**\
  A boolean expressing whether this resource is ephemeral or not, i.e., exists only during a transaction.
* **`nonce`**\
  A number ensuring the uniqueness of the resource by preventing commitment hash collisions.
* **`randSeed`**\
  A number to derive (pseudo)-randomness from that might be used in apps.

Further information can be found on the [Anoma specs page](https://specs.anoma.net/latest/arch/system/state/resource_machine/index.html).
