---
icon: rocket-launch
description: >-
  The following sets you up to build, run, and test your first Anoma
  application.
---

# Your First Anoma App

## Prerequisites

Before creating your first Anoma dApp, make sure to have [Juvix installed](../../further-resources/juvix/install-juvix.md).

Additionally, you can learn how to [write a Juvix project](../../further-resources/juvix/write-a-juvix-project.md) to nail the very basics of coding in Juvix.

## Building `HelloWorld`&#x20;

The goal of this tutorial is to&#x20;

1. Define a "Hello World!" [Resource](../../learn/resources/).
2. Add a [resource label](../../learn/resources/#label).
3. Write a [transaction function](../../learn/applications/interface.md#transaction-functions) initializing the resource.

### Project Setup

Let's fly through the project setup by typing the following in our terminal.

{% code title="~/" %}
```bash
mkdir HelloWorld
cd HelloWorld
juvix init
# (1) Next, type 'hello-world' for the project name
# (2) Finally, press 'Enter' to choose the default version number
```
{% endcode %}

In the following chapter, we're going to construct the scaffolding of the [resource object](../../learn/resources/resource-object.md) and add our custom label.
