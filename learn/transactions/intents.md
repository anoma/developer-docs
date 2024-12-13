---
description: This page explains how intents and solving work.
---

# Intents

I**ntents** are unbalanced transactions requiring matching unbalanced transaction by one or multiple counterparties.&#x20;

Anoma users submit their intents to an **intent pool** in the form of unbalanced transactions, which are received and processed by [**solvers**](../services/solving.md) that output balanced ARM transactions. These transactions are then ordered and finally sent to the executor node, that verifies and executes the transactions in the determined order, updating the state.

Below, we show examples of a balanced transaction that can directly be executed and an intent (unbalanced transaction) requiring counterparty discovery.

{% tabs %}
{% tab title="Transfer" %}
#### **Alice's balanced transaction**

| Consume       | Create     |
| ------------- | ---------- |
| 1🍏 `{Alice}` | 1🍏`{Bob}` |

Alice's consumes here apple resource and creates one with Bob as the owner. This transaction is already balanced and therefore requires no solving. It can be executed straight away.

_(Names in curly braces indicate the resource owner.)_
{% endtab %}

{% tab title="Basic Intent" %}
**Alice's unbalanced txn (Intent)**

| Consume       | Create       |
| ------------- | ------------ |
| 1🍏 `{Alice}` | 1🥖`{Alice}` |

#### **Bob's unbalanced txn (Intent)**

| Consume     | Create      |
| ----------- | ----------- |
| 1🥖 `{Bob}` | 1🍏 `{Bob}` |

#### Balanced txn (obtained by composition of Alice's and Bob's unbalanced txns)

| Consume       | Create        |
| ------------- | ------------- |
| 1🍏 `{Alice}` | 1🍏 `{Bob}`   |
| 1🥖 `{Bob}`   | 1🥖 `{Alice}` |

After composing Alice's and Bob's transactions into one, the transaction is now balanced and can be executed. After execution, Alice has swapped her apple for a baguette, while Bob has done it the other way around.\
\
&#xNAN;_(Names in curly braces indicate the resource owner.)_
{% endtab %}
{% endtabs %}



