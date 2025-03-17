---
description: This page describes the Action object and its purpose.
---

# Actions

Actions separate the transaction, i.e., consumed and created resources and related data, into distinct contexts. For example, one action can transfer coins from Alice to Bob, whereas another one transfers coins from Carol to Dave.&#x20;

Resource logics of created and consumed resources can only see and relate to other resources if they are part of the same action. This **context separation** is important to compose transactions with each other without causing interferences.&#x20;

The underlying action object contains standardized data fields, which are defined in the [Anoma specs](https://specs.anoma.net/latest/arch/system/state/resource_machine/data_structures/action/index.html).
