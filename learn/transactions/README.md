---
icon: arrow-right-arrow-left
description: This page introduces transaction transitioning the Anoma state upon execution.
---

# Transactions

Transactions transition the Anoma state by consuming and creating resources when they are executed.&#x20;

## Transaction Delta

Transactions with equal quantities of created and consumed resources of the same [kind](../resources/#resource-kind) are called **balanced**. A balanced transaction has a **delta** value of 0. Furthermore, a transaction is **valid** if all resource logic and compliance proofs are valid (see the [Anoma resource machine](../page/#transaction-checks)). Only balanced and valid transactions can be executed. \
Nodes send balanced transactions to a **transaction mempool**, where they can be picked up by a block producer that verifies and executes the transactions in the determined order, thus  updating the state.

A transaction with a non-zero **delta** value is called **unbalanced**. Unbalanced transactions are [**intents**](intents.md) and require counterparties to add consumed and created resources to the transaction to balance it.

Test your understanding about balanced transactions  by completing the exercise below.

{% tabs %}
{% tab title="Exercise" %}
**Is the transaction balanced? Answer for each row.**

<table><thead><tr><th align="center">Consumed</th><th align="center">Created</th><th data-type="checkbox">Answer</th></tr></thead><tbody><tr><td align="center">2🍏</td><td align="center">2🍏</td><td>false</td></tr><tr><td align="center">2🍏</td><td align="center">1🍏 + 1🍏</td><td>false</td></tr><tr><td align="center">1🍏</td><td align="center">1🐚</td><td>false</td></tr><tr><td align="center">2🍏 + 1🐚</td><td align="center">1🍏 + 2🐚</td><td>false</td></tr><tr><td align="center">2🍏 + 1🐚</td><td align="center">1🍏 + 1🍎 + 1🐚</td><td>false</td></tr><tr><td align="center">1🍏 + 2🍎 + 2🍎 + 3🐚</td><td align="center">1🍏 + 4🍎 + 1🐚 + 2🐚</td><td>false</td></tr></tbody></table>
{% endtab %}

{% tab title="Solution" %}
**Is the transaction balanced? Answer for each row.**

<table><thead><tr><th align="center">Consumed</th><th align="center">Created</th><th data-type="checkbox"></th></tr></thead><tbody><tr><td align="center">2🍏</td><td align="center">2🍏</td><td>true</td></tr><tr><td align="center">2🍏</td><td align="center">1🍏 + 1🍏</td><td>true</td></tr><tr><td align="center">1🍏</td><td align="center">1🐚</td><td>false</td></tr><tr><td align="center">2🍏 + 1🐚</td><td align="center">1🍏 + 2🐚</td><td>false</td></tr><tr><td align="center">2🍏 + 1🐚</td><td align="center">1🍏 + 1🍎 + 1🐚</td><td>false</td></tr><tr><td align="center">1🍏 + 2🍎 + 2🍎 + 3🐚</td><td align="center">1🍏 + 4🍎 + 1🐚 + 2🐚</td><td>true</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

Next, we will look at the transaction object data structure in detail.
