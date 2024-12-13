---
icon: circle
description: >-
  This page introduces resources, the state and logic they carry, and their
  lifecycle in the Anoma protocol.
---

# Resources

Resources are **atomic units of state and logic**. In the following, we give an overview of the resource state, resource logics, data that can be derived from resources, as well as their lifecycle in the Anoma protocol.

## State

Resources are associated with state, i.e., a **label**, a **quantity**, and a **value**.

### Label

The label can contain arbitrary data describing the resource and determining its [kind](./#resource-kind). Examples for label data are the name and symbol of a currency, the species of an apple, or a message associated with a specific channel in a messaging application.

### Value

The value field can contain arbitrary data associated with this specific resource.  Examples for value data are information about the owner, the fruitiness of a fruit, or the text in a message. The difference to the label field is that this data is not influencing the [resource kind](./#resource-kind).

### Quantity

The quantity indicates the number of units that the resource represents. In practice, resources are often split and joined. For example, Alice owning a 10 USD resource might want to give 4 USD to pay someone and keep the remaining 6 for herself. In some cases, only one resource instance should ever exist (e.g., for a message or an NFT).

| 10 💵 `{owner : 0x123...}` | 2 🍏 `{fruitiness : 8 / 10}` | 1 💌 `{text : "I ❤️ u"}` |
| :------------------------: | :--------------------------: | ------------------------ |
|      `10 USD` resource     |    `2 GreenApple` resource   | `1 Message` Resource     |

There are more fields that are part of the resource object and make up its state. To learn about them, visit the [resource object](resource-object.md) section.

## Logic

Besides state, resources are associated with a **logic function**. The logic function enforces predicates checking data, e.g., in the resource itself, the transaction or resources in the same transaction context.

|                                         💵 Logic                                        |                          🍏 Logic                         |  💌  Logic                                                           |
| :-------------------------------------------------------------------------------------: | :-------------------------------------------------------: | -------------------------------------------------------------------- |
| This resource can be transferred if the owner signs a message specifying the new owner. | This apple can be eaten if the fruitiness is at least 3.  | This message can be edited by the author being encoded in the label. |

## Kind

The _kind_ of a resource determines its fungibility and is computed as the hash of its _logic_ and _label_.

$$
\texttt{kind} := h_\texttt{kind}(\texttt{logic},\,\texttt{label})
$$

The kind is used to check if transactions are balanced (which will be explained in the [resource machine](../page/) section) and is a requirement for a transaction to be executed.

## Lifecycle

Resources have a lifecycle with three stages:

{% stepper %}
{% step %}
### Non-existent

A resource is non-existent when its commitment hasn't been added to the controller that the resource lives on.
{% endstep %}

{% step %}
### Created

A resource is created after its commitment was added in the Merkle tree (see [#creation](./#creation "mention")) of the controller (e.g., blockchain) the resource will live on.
{% endstep %}

{% step %}
### Consumed

A resource is consumed after its nullifier was added to the nullifier set (see [#consumption](./#consumption "mention")) of the controller (e.g., blockchain) the resource lived on.
{% endstep %}
{% endstepper %}

### Creation

To create a resource, its _commitment_ must be computed by hashing the resource object.&#x20;

$$
\texttt{commitment} := h_\texttt{cm}(\texttt{resource})
$$

The commitment is then put into a [transaction](../transactions/). After execution, the commitment is added to a Merkle tree.

### **Consumption**

To consume a resource, its _nullifier_ must be computed by hashing the resource object and a secret called the _nullifier key_.

$$
\texttt{nullifier} := h_\texttt{nf}(\texttt{resource},\,\texttt{nullifierKey})
$$

The nullifier is then put into a [transaction](../transactions/).  After execution, the nullifier is added to a nullifier set. This nullification mechanism makes the consumption of the resource unlinkable to its past creation.

### **Ephemeral Resources**

In some cases, it is required to create resources that just exist over the course of the transaction. These resources are called **ephemeral** and can be identified by a dedicated flag in the [resource object data structure](resource-object.md). This is often used to balance out transactions that otherwise would be unbalanced, for example, when resources are initially created or finally consumed. Another use case is to write advanced [intents](../transactions/intents.md) that express optionality or have more sophisticated constraints. The latter is achieved by creating an ephemeral intent resource expressing the preferred state transition and constraints that can then be matched and consumed by a solver.
