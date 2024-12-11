---
icon: arrow-right-arrow-left
---

# Transactions

**Transactions** consume and create resources.&#x20;

* Transactions with equal quantities of created and consumed resources of the same kind are **balanced**.
* A transaction is **valid** if all resource logics are valid.
* Only balanced and valid transactions can be executed.
* Contain **actions** for context separation.

Test your understanding by completing the exercise below.

{% tabs %}
{% tab title="Exercise" %}
**Is the transaction balanced? Answer for each row.**

<table><thead><tr><th align="center">Consumed</th><th align="center">Created</th><th data-type="checkbox">Answer</th></tr></thead><tbody><tr><td align="center">2🍏</td><td align="center">2🍏</td><td>false</td></tr><tr><td align="center">2🍏</td><td align="center">1🍏 + 1🍏</td><td>false</td></tr><tr><td align="center">2🍏 + 1🐚</td><td align="center">1🍏 + 2🐚</td><td>false</td></tr><tr><td align="center">2🍏 + 1🐚</td><td align="center">1🍏 + 1🍎 + 1🐚</td><td>false</td></tr><tr><td align="center">1🍏 + 2🍎 + 2🍎 + 3🐚</td><td align="center">1🍏 + 4🍎 + 1🐚 + 2🐚</td><td>false</td></tr></tbody></table>
{% endtab %}

{% tab title="Solution" %}
**Is the transaction balanced? Answer for each row.**

<table><thead><tr><th align="center">Consumed</th><th align="center">Created</th><th data-type="checkbox"></th></tr></thead><tbody><tr><td align="center">2🍏</td><td align="center">2🍏</td><td>true</td></tr><tr><td align="center">2🍏</td><td align="center">1🍏 + 1🍏</td><td>true</td></tr><tr><td align="center">2🍏 + 1🐚</td><td align="center">1🍏 + 2🐚</td><td>false</td></tr><tr><td align="center">2🍏 + 1🐚</td><td align="center">1🍏 + 1🍎 + 1🐚</td><td>false</td></tr><tr><td align="center">1🍏 + 2🍎 + 2🍎 + 3🐚</td><td align="center">1🍏 + 4🍎 + 1🐚 + 2🐚</td><td>true</td></tr></tbody></table>
{% endtab %}
{% endtabs %}

Next, we will look at the transaction object data structure in detail.
