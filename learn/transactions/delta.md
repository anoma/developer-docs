---
description: This page explains how the concept of the transaction balance.
---

# Delta

The transaction **delta** value is computed by adding and subtracting the quantities of created and consumed resources of the same kind, respectively.

Transactions with a zero delta value are [**balanced transactions**](balanced-transactions.md).\
Transactions with a **non-zero delta** value are **unbalanced transactions** a.k.a. [**intents**](intents.md).&#x20;

Test your understanding about the transaction delta by completing the exercise below.

{% tabs %}
{% tab title="Exercise" %}
**Is the transaction balanced? Answer for each row.**

<table><thead><tr><th align="center">Consumed</th><th align="center">Created</th><th data-type="checkbox">Answer</th></tr></thead><tbody><tr><td align="center"><kbd>2🍏</kbd></td><td align="center"><kbd>2🍏</kbd></td><td>false</td></tr><tr><td align="center"><kbd>2🍏</kbd></td><td align="center"><kbd>1🍏</kbd> <kbd>1🍏</kbd></td><td>false</td></tr><tr><td align="center"><kbd>1🍏</kbd></td><td align="center"><kbd>1🐚</kbd></td><td>false</td></tr><tr><td align="center"><kbd>2🍏</kbd> <kbd>1🐚</kbd></td><td align="center"><kbd>1🍏</kbd>  <kbd>1🐚</kbd></td><td>false</td></tr><tr><td align="center"><kbd>2🍏</kbd> <kbd>1🐚</kbd></td><td align="center"><kbd>1🍏</kbd> <kbd>1🍎</kbd> <kbd>1🐚</kbd></td><td>false</td></tr><tr><td align="center"><kbd>1🍏</kbd> <kbd>2🍎</kbd> <kbd>2🍎</kbd> <kbd>3🐚</kbd></td><td align="center"><kbd>1🍏</kbd> <kbd>4🍎</kbd> <kbd>1🐚</kbd> <kbd>2🐚</kbd></td><td>false</td></tr></tbody></table>
{% endtab %}

{% tab title="Solution" %}
**Is the transaction balanced? Answer for each row.**

<table><thead><tr><th align="center">Consumed</th><th align="center">Created</th><th data-type="checkbox">Answer</th></tr></thead><tbody><tr><td align="center"><kbd>2🍏</kbd></td><td align="center"><kbd>2🍏</kbd></td><td>true</td></tr><tr><td align="center"><kbd>2🍏</kbd></td><td align="center"><kbd>1🍏</kbd> <kbd>1🍏</kbd></td><td>true</td></tr><tr><td align="center"><kbd>1🍏</kbd></td><td align="center"><kbd>1🐚</kbd></td><td>false</td></tr><tr><td align="center"><kbd>2🍏</kbd> <kbd>1🐚</kbd></td><td align="center"><kbd>1🍏</kbd>  <kbd>1🐚</kbd></td><td>false</td></tr><tr><td align="center"><kbd>2🍏</kbd> <kbd>1🐚</kbd></td><td align="center"><kbd>1🍏</kbd> <kbd>1🍎</kbd> <kbd>1🐚</kbd></td><td>false</td></tr><tr><td align="center"><kbd>1🍏</kbd> <kbd>2🍎</kbd> <kbd>2🍎</kbd> <kbd>3🐚</kbd></td><td align="center"><kbd>1🍏</kbd> <kbd>4🍎</kbd> <kbd>1🐚</kbd> <kbd>2🐚</kbd></td><td>true</td></tr></tbody></table>
{% endtab %}
{% endtabs %}
