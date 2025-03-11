---
description: >-
  This page explains the resource lifecycle and computable components and
  properties associated with it
---

# Lifecycle

Resources have a lifecycle with three stages:

{% stepper %}
{% step %}
### Non-existent

A resource is non-existent when its commitment hasn't been added to the controller that the resource lives on.
{% endstep %}

{% step %}
### Created

A resource is created after its commitment was added in the Merkle tree of the controller (e.g., blockchain) the resource will live on.
{% endstep %}

{% step %}
### Consumed

A resource is consumed after its nullifier was added to the nullifier set of the controller (e.g., blockchain) the resource lived on.
{% endstep %}
{% endstepper %}

## Creation

To create a resource, its **commitment** must be computed by hashing the resource object.&#x20;

$$
\texttt{commitment} := h_\texttt{cm}(\texttt{resource})
$$

The commitment is then put into a [transaction](../transactions/). After execution, the commitment is added to a Merkle tree.

## **Consumption**

To consume a resource, its **nullifier** must be computed by hashing the resource object and a secret called the **nullifier key**.

$$
\texttt{nullifier} := h_\texttt{nf}(\texttt{resource},\,\texttt{nullifierKey})
$$

The nullifier is then put into a [transaction](../transactions/).  After execution, the nullifier is added to a nullifier set. This nullification mechanism makes the consumption of the resource unlinkable to its past creation.

{% hint style="info" %}
**Current private devnet**\
The current devnet supports only the [transparent ARM](../page/information-flow-control.md#resource-machine-instances) in which the nullifier key is always 0.
{% endhint %}

## **Ephemeral Resources**

In some cases, it is required to create resources that just exist over the course of the transaction. These resources are called **ephemeral** and can be identified by a dedicated flag in the [resource object data structure](https://specs.anoma.net/latest/arch/system/state/resource_machine/data_structures/resource/index.html). This is often used to balance out transactions that otherwise would be unbalanced, for example, when resources are initially created or finally consumed. Another use case is to write advanced [intents](../transactions/intents.md) that express optionality or have more sophisticated constraints. The latter is achieved by creating an ephemeral intent resource expressing the preferred state transition and constraints that can then be matched and consumed by a solver.
