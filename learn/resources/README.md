---
icon: circle
---

# Resources

Resources are atomic units of application state and logic. In the following, we give an overview about&#x20;

## Overview: Resource State

Resources are associated with state, i.e., a **label**, a **quantity** and a **value**.

The **label** can contain arbitrary data describing the resource and determining its [kind](./#resource-kind). Examples for label data are the name and symbol of a currency, the species of an apple, or a message associated with a specific channel in a messaging application.

The **value** field can contain arbitrary data associated with this specific resource.  Examples for value data are information about the owner, the fruitiness of a fruit, or the text in a message. The difference to the label field is that this data is not influencing the [resource kind](./#resource-kind).

The **quantity** indicates the number of units that the resource represents. In practice, resources are often split and joined. For example, Alice owning a 10 USD resource might want to give 4 USD to pay someone and keep the remaining 6 for herself. In some cases, only one resource instance should ever exist (e.g., for a message or an NFT).

| 10 💵 `{owner : 0x123...}` | 2 🍏 `{fruitiness : 8 / 10}` | 1 💌 `{text : "I ❤️ u"}` |
| :------------------------: | :--------------------------: | ------------------------ |
|      `10 USD` resource     |    `2 GreenApple` resource   | `1 Message` Resource     |

In the [resource-object.md](resource-object.md "mention")section, you will learn more details about the resource object and its state.

## Overview: Resource Logic

Besides state, resources are associated with a logic function. The logic function enforces predicates (that check data, e.g., in the resource itself, the transaction or resources in the same transaction context)

|                                           💵 Logic                                           |                            🍏 Logic                           |  💌  Logic                                                          |
| :------------------------------------------------------------------------------------------: | :-----------------------------------------------------------: | ------------------------------------------------------------------- |
| This resource can only be transferred if the owner signs a message specifying the new owner. | This apple can only be eaten if the fruitness is at least 3.  | This message can only be edited by the author encoded in the label. |



## Resource Kind

The _kind_ of a resource determines its fungibility and is computed as the hash of its _logic_ and _label_.

$$
\texttt{kind} := h_\texttt{kind}(\texttt{logic},\,\texttt{label})
$$

The kind is used to check if transactions are balanced (which will be explained in the [resource machine](../page/) setion) and is a requirement for a transaction to be executed.

## Lifecyle

Resources have a lifecycle with three stages:

{% stepper %}
{% step %}
### Non-existent

A resource is non-existent when its commitment hasn't been added to the controller the resource lives on.
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

To consume a resource its _nullifier_ must be computed by hashing the resource object and a secret called the _nullifier key_.

$$
\texttt{nullifier} := h_\texttt{nf}(\texttt{resource},\,\texttt{nullifierKey})
$$

The nullifier is then put into a [transaction](../transactions/).  After execution, the nullifier is added to a nullifier set. This nullification mechanism makes the consumption of the resource unlinkable to its past creation.
