---
description: >-
  This page explains the application interface which is constituted by
  projection and transaction functions.
---

# Interface

The application interface consists of two components:

1. **Transaction functions** representing the **application** **write interface**.
2. **Projection functions** representing the **application** **read interface**.

## Transaction Functions&#x20;

Transaction functions take arbitrary input data and output a [transaction object](../transactions/transaction-object.md). Besides populating the transaction object with the required data[^1], they also take care of checking the input arguments for correctness and can return error messages.

Transaction function examples are:

```agda
transfer (caller receiver : Identity) (toTransfer : Resource) : Transaction
```

* A `transfer` function consuming an owned resource and creating one owned by a receiver.

```agda
merge (caller : Identity) : (toMerge : List Resource) : Transaction
```

* A `merge` function merging a list of resources into one.

```agda
swap (caller : Identity) (give want : List Resources) : Transaction
```

* A `swap` function consuming a list of resources and specifying a list of resource kinds and quantities that the caller wants to receive in return.

## Projection Functions

Projection functions project data from the state (usually being fragmented into many different resources) into a format being useful to application users. As such, they take a list of resources as input and output arbitrary data.&#x20;

Projection function examples are:

```agda
totalBalance (ownedTokens : List Resources) : Quantity
```

* A `totalBalance` function returning the total quantity of resources of a specific kind being owned by an owner identity.

```agda
chatHistory (messages : List Resource) : List String
```

* A `chatHistory` function returning the message texts of a message channel.

Commonly, the input resources passed to projection functions are obtained after indexing and custom-filtering, e.g., from an [indexing service provider](../services/indexing.md). &#x20;

[^1]: I.e., actions as well as commitments, nullifiers, and application data therein, proofs, the transaction delta value and state roots.
