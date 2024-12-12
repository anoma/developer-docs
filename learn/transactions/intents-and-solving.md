---
description: This page explains how intents and solving work.
---

# Intents & Solving

I**ntents** are unbalanced transactions requiring matching unbalanced transaction by one or multiple counterparties. This works as follows.

Anoma users submit their intents to an **intent gossip network** in the form of unbalanced transactions, which are received and processed by **solvers** that output balanced ARM transactions. These transactions are then ordered and finally sent to the executor node, that verifies and executes the transactions in the determined order, updating the state.

{% hint style="info" %}
For the current private devnet, the Anoma node is running a brute-force solver. The solver tries to find matching intents in the intent pool by composing them and checking if the resulting transaction is valid.
{% endhint %}

{% tabs %}
{% tab title="Transfer" %}
#### **Alice's balanced transaction**

| Consume               | Create             |
| --------------------- | ------------------ |
| 1🍏 `{owner : Alice}` | 1🍏`{owner : Bob}` |

This transaction is balanced already, requires no solving, and can be executed straight away.
{% endtab %}

{% tab title="Basic Intent" %}
#### **Alice's unbalanced txn (Intent)**

| Consume               | Create               |
| --------------------- | -------------------- |
| 1🍏 `{owner : Alice}` | 1🥖`{owner : Alice}` |

#### **Bob's unbalanced txn (Intent)**

| Consume             | Create              |
| ------------------- | ------------------- |
| 1🥖 `{owner : Bob}` | 1🍏 `{owner : Bob}` |

#### Balanced txn (obtained by composition of Alice's and Bob's unbalanced txns)

| Consume               | Create                |
| --------------------- | --------------------- |
| 1🍏 `{owner : Alice}` | 1🍏 `{owner : Bob}`   |
| 1🥖 `{owner : Bob}`   | 1🥖 `{owner : Alice}` |

After composing Alice's and Bob's transactions into one, the transaction is now balanced and can be executed. After execution, Alice has swapped her apple for a baguette, while Bob has done it the other way around.
{% endtab %}
{% endtabs %}
