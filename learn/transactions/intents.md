---
description: This page explains how intents and solving works within the Anoma protocol.
---

# Intents

**Intents**, which are **unbalanced transactions**, can be become balanced transactions through composition with matching intents by other counterparties.

Anoma users submit their intents to an **intent pool** in the form of unbalanced transactions, which are received and processed by [solvers](../services/solving.md) that output [balanced transactions](balanced-transactions.md). These transactions are then ordered and finally sent to the executor node, which verifies and executes the transactions in the determined order, updating the state.

Below, we show examples of a balanced transaction that can directly be executed and two flavors of  intents (unbalanced transactions) requiring counterparty discovery.

{% hint style="info" %}
**Legend**

* Symbols, e.g., <kbd>🍏</kbd> , indicate resource objects that can contain arbitrary data and logic.
* Numbers Preceeding numbers, e.g., <kbd>4🍏</kbd> indicate the quantity of resources.
* Names following a resource, e.g., <kbd>4🍏 Alice</kbd> , indicate who is authorized to consume it.
* <kbd><mark style="color:blue;">Blue<mark style="color:blue;"></kbd> coloring indicates ephemeral resources.&#x20;
* Speech bubbles <kbd>💬</kbd> indicate ephemeral intent resources expressing constraints over the transaction.
{% endhint %}

{% tabs %}
{% tab title="Basic Intent" %}
Alice owns apple <kbd>🍏</kbd> and Bob owns banana <kbd>🍌</kbd> resources. Both want to trade their fruits and know exactly what they want. Both don't need to know each other.

#### Alice's unbalanced txn (intent)

"I want to give 3🍏 for 2🍌."

| Consume              | Create               |
| -------------------- | -------------------- |
| <kbd>3🍏 Alice</kbd> | <kbd>2🍌 Alice</kbd> |

#### Bob's unbalanced txn (intent)

"I want to give 2🍌 for 3🍏."

| Consume            | Create             |
| ------------------ | ------------------ |
| <kbd>2🍌 Bob</kbd> | <kbd>3🍏 Bob</kbd> |

#### Balanced txn (composition of Alice's and Bob's unbalanced txns)

Anyone seeing the two transactions (including Alice and Bob themselves) can compose the unbalanced transactions to obtain a balanced transaction containing two actions.

| Consume              | Create               |
| -------------------- | -------------------- |
| <kbd>3🍏 Alice</kbd> | <kbd>2🍌 Alice</kbd> |

```c
// Action Separator
```

| <kbd>2🍌 Bob</kbd> | <kbd>3🍏 Bob</kbd> |
| ------------------ | ------------------ |

After execution,

* Alice has swapped her <kbd>3🍏 Alice</kbd> resource for a <kbd>2🍌 Alice</kbd> resource,&#x20;
* Bob has swapped his  <kbd>2🍌 Bob</kbd> for a <kbd>3🍏 Bob</kbd> resource.

This outcome is equivalent to two balanced transactions (see the previous tab) where both, Alice and Bob, transfer resources to one another.
{% endtab %}

{% tab title="Advanced Intent & Solution" %}
Alice owns apple <kbd>🍏</kbd> and Bob owns banana <kbd>🍌</kbd> resources. Both want to trade some of their fruits within specific constraints. Both don't know each other.

Both, Alice and Bob express their intents over the preferred state transition as ephemeral intent resources <kbd>💬</kbd> to solver Sally.

#### Alice's unbalanced txn (intent)

"Sally, I want to give exactly 4🍏 for at least 4🍌."

<table data-full-width="true"><thead><tr><th>Consume</th><th>Create</th></tr></thead><tbody><tr><td><kbd>4🍏 Alice</kbd></td><td><kbd><mark style="color:blue;">1💬 Sally, give = 4🍏 Alice, want ≥ 4🍌 Alice</mark></kbd></td></tr></tbody></table>

#### Bob's unbalanced txn (intent)

"Sally, I want to give at most 7🍌 for exactly 3🍏."

<table data-full-width="true"><thead><tr><th>Consume</th><th>Create</th></tr></thead><tbody><tr><td><kbd>7🍌 Bob</kbd></td><td><kbd><mark style="color:blue;">1💬 Sally, give ≤ 7🍌 Bob, want = 3🍏 Bob</mark></kbd></td></tr></tbody></table>

#### Solver Sally's unbalanced txn (solution)

Seeing both intents in the intent pool, Sally comes up with a solution for the two intents:

"I'll to give 3🍏 to Bob and take 1🍏 for myself as a fee. Furthermore, I'll give 5🍌 to Alice, return 1🍌 to Bob, and  take 1🍌 for myself as a fee."

<table data-full-width="true"><thead><tr><th width="351">Consume</th><th>Create</th></tr></thead><tbody><tr><td><kbd><mark style="color:blue;">1💬 Sally, give = 4🍏 Alice, want ≥ 4🍌 Alice</mark></kbd></td><td><kbd>3🍏 Bob</kbd></td></tr><tr><td><kbd><mark style="color:blue;">1💬 Sally, give ≤ 7🍌 Bob, want = 3🍏 Bob</mark></kbd></td><td><kbd>1🍏 Sally</kbd></td></tr><tr><td></td><td><kbd>5🍌 Alice</kbd></td></tr><tr><td></td><td><kbd>1🍌 Bob</kbd></td></tr><tr><td></td><td><kbd>1🍌 Sally</kbd></td></tr></tbody></table>

#### Balanced txn (composition of Alice's, Bob's, and Sally's unbalanced txns)

Sally composes her solution with Alice's and Bob's intents and obtains a balanced transaction containing three actions.

<table data-full-width="true"><thead><tr><th>Consume</th><th>Create</th></tr></thead><tbody><tr><td><kbd>4🍏 Alice</kbd></td><td><kbd><mark style="color:blue;">1💬 Sally, give = 4🍏 Alice, want ≥ 4🍌 Alice</mark></kbd></td></tr></tbody></table>

{% code fullWidth="true" %}
```c
// Action Separator
```
{% endcode %}

<table data-header-hidden data-full-width="true"><thead><tr><th>Consume</th><th>Create</th></tr></thead><tbody><tr><td><kbd>7🍌 Bob</kbd></td><td><kbd><mark style="color:blue;">1💬 Sally, give ≤ 7🍌 Bob, want = 3🍏 Bob</mark></kbd></td></tr></tbody></table>

```c
// Action Separator
```

<table data-header-hidden data-full-width="true"><thead><tr><th>Consume</th><th>Create</th></tr></thead><tbody><tr><td><kbd><mark style="color:blue;">1💬 Sally, give = 4🍏 Alice, want ≥ 4🍌 Alice</mark></kbd></td><td><kbd>3🍏 Bob</kbd></td></tr><tr><td><kbd><mark style="color:blue;">1💬 Sally, give ≤ 7🍌 Bob, want = 3🍏 Bob</mark></kbd></td><td><kbd>1🍏 Sally</kbd></td></tr><tr><td></td><td><kbd>5🍌 Alice</kbd></td></tr><tr><td></td><td><kbd>1🍌 Bob</kbd></td></tr><tr><td></td><td><kbd>1🍌 Sally</kbd></td></tr></tbody></table>

After execution,

* Alice gave her <kbd>4🍏</kbd>  and got <kbd>5🍌</kbd> , more than the least amount she specified,
* Bob gave <kbd>6🍌</kbd> and has <kbd>1🍌</kbd>left, less than max amount he specified, and got the <kbd>3🍏</kbd>  apples he wanted,
* Sally took <kbd>1🍏</kbd> and <kbd>1🍌</kbd> for her services. If she is taking too much for herself, users might decide to not let her settle their intents anymore (see the [Slow Games ART](https://zenodo.org/records/13765214)).
{% endtab %}
{% endtabs %}
