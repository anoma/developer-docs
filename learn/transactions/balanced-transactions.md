---
description: This page explains balanced transactions and how they can be formed.
---

# Balanced Transactions

Balanced transactions are characterized by a zero [transaction delta](delta.md). They can be executed if they are valid and comply with the [resource machine rules](../page/). One way of obtaining balanced transactions  is through composition of matching, [unbalanced transactions a.k.a. as intents](intents.md). However, a single party can often directly produce a balanced transaction without requiring any counterparty.

In the following, we show examples of transactions that can be sent by a single party, here Alice.

{% hint style="info" %}
**Legend**

* Symbols, e.g., <kbd>🍏</kbd> , indicate resource objects that can contain arbitrary data and logic.
* Numbers Preceeding numbers, e.g., <kbd>4🍏</kbd> indicate the quantity of resources.
* Names following a resource, e.g., <kbd>4🍏 Alice</kbd> , indicate who is authorized to consume.
* <kbd><mark style="color:blue;">Blue<mark style="color:blue;"></kbd>  coloring indicates ephemeral resources.
{% endhint %}

## Resource Property Changes

Often, transactions and actions within aim to change one or multiple properties of existing resource objects. Most of the time, a single, authorized party (i.e., the owner) can unilaterally execute the transaction. Actions can include, for example,&#x20;

* Transferring a resource, which changes the owner-property
* Splitting and merging resources, which changes the quantity property

### Examples

{% tabs %}
{% tab title="Transfer" %}
Alice owns an apple resource <kbd>🍏</kbd> and wants to transfer it to Bob.

#### **Alice's balanced transaction**

Alice consumes her <kbd>3🍏 Alice</kbd> resource and creates a <kbd>3🍏 Bob</kbd> resource with Bob as the owner.

| Consume              | Create             |
| -------------------- | ------------------ |
| <kbd>3🍏 Alice</kbd> | <kbd>3🍏 Bob</kbd> |

After execution, Alice has transferred <kbd>3🍏</kbd> to Bob but the total quantity hasn't changed.
{% endtab %}

{% tab title="Split" %}
Alice owns an <kbd>3🍏</kbd> apple resource and wants to split it into a <kbd>2🍏</kbd> and a <kbd>1🍏</kbd> resource.

#### **Alice's balanced transaction**

Alice consumes her <kbd>3🍏 Alice</kbd> resource and creates a <kbd>2🍏 Alice</kbd> and a  <kbd>1🍏 Alice</kbd> resource for herself.

| Consume              | Create               |
| -------------------- | -------------------- |
| <kbd>3🍏 Alice</kbd> | <kbd>2🍏 Alice</kbd> |
|                      | <kbd>1🍏 Alice</kbd> |

After execution, Alice has two resource objects instead of one, but the total quantity hasn't changed.
{% endtab %}

{% tab title="Merge" %}
Alice owns a <kbd>2🍏</kbd> and a <kbd>1🍏</kbd> resource and wants to merge them into one <kbd>3🍏</kbd> resource.

#### **Alice's balanced transaction**

Alice consumes her <kbd>3🍏 Alice</kbd> resource and creates a <kbd>2🍏 Alice</kbd> and a  <kbd>1🍏 Alice</kbd> resource for herself.

| Consume              | Create               |
| -------------------- | -------------------- |
| <kbd>2🍏 Alice</kbd> | <kbd>3🍏 Alice</kbd> |
| <kbd>1🍏 Alice</kbd> |                      |

After execution,  Alice has one resource object instead of two, but the total quantity hasn't changed.
{% endtab %}
{% endtabs %}

## Resource Supply Changes

Changing resource properties requires resource to exist in the first place. However, immediately after the genesis of a resource controller (e.g., the launch of a new blockchain or the deployment of a protocol adapter) the associated commitment tree is empty. The question is how resources can initially created but also finally consumed.

We call the process of initially creating a resource out of nothing **initialization**, and the process of finally consuming a resource **finalization**.\
The former and latter result in **in- and deflation of the supply** (i.e.,  the total quantity) of the associated resource kind, respectively. Both require utilizing [ephemeral resources](../resources/#ephemeral-resources) to balance the transaction.

{% hint style="info" %}
**Constraining the Supply**

Resources logics can include constraints and mechanisms fixing the **supply** (i.e., the total quantity of all resources) of a given resource kind after initialization or allowing only a specific **originator** identity to in- or deflate the supply.
{% endhint %}

### Examples

{% tabs %}
{% tab title="Initialization" %}
Alice wants to initiallly create an apple resource (without consuming an existing one) and inflate the total <kbd>🍏</kbd> supply by 3. To balance the transaction she uses an [ephemeral resource](../resources/#ephemeral-resources).

#### **Alice's balanced transaction**

Alice consumes an ephemeral <kbd><mark style="color:blue;">3🍏 Alice<mark style="color:blue;"></kbd> resource and creates a non-ephemeral <kbd>3🍏 Alice</kbd> resource.

| Consume                                                                  | Create               |
| ------------------------------------------------------------------------ | -------------------- |
| <kbd><mark style="color:blue;">3🍏 Alice<mark style="color:blue;"></kbd> | <kbd>3🍏 Alice</kbd> |

After execution, Alice has created <kbd>3🍏</kbd> resources for herself.
{% endtab %}

{% tab title="Finalization" %}
Alice wants to finally consume an apple resource <kbd>🍏</kbd> (without creating a new one) to deflate the total <kbd>🍏</kbd> supply. To balance the transaction she uses an [ephemeral resource](../resources/#ephemeral-resources).

#### **Alice's balanced transaction**

Alice consumes a non-ephemeral <kbd>3🍏 Alice</kbd> resource and creates an ephemeral <kbd><mark style="color:blue;">3🍏 Alice<mark style="color:blue;"></kbd> resource.

| Consume              | Create                                                                   |
| -------------------- | ------------------------------------------------------------------------ |
| <kbd>3🍏 Alice</kbd> | <kbd><mark style="color:blue;">3🍏 Alice<mark style="color:blue;"></kbd> |

This transaction is already balanced and therefore requires no solving. It can be executed straight away. After execution, Alice has consumed <kbd>3🍏</kbd> resources for herself.
{% endtab %}
{% endtabs %}

[Applications](../applications/) provide so-called [transaction functions](../applications/interface.md#transaction-functions) as part of their interface to create [transaction objects](broken-reference).
