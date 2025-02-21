---
icon: rocket-launch
description: >-
  A tutorial setting you up to build, run, and test your first Anoma
  application.
---

# Your First Anoma App

## Prerequisites

Before creating your first Anoma dApp, make sure to have [Juvix installed](../getting-started.md).

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
```
{% endcode %}

In the following chapter, we're going to construct the scaffolding of the [resource object](../../learn/resources/resource-object.md) and add our custom label.

Once the Juvix project is created, we want to change the `Package.juvix` file to the following:

{% code title="HelloWorld/" %}
```agda
module Package;

import PackageDescription.V2 open;

package : Package :=
  defaultPackage@{
    name := "simple-hello-world";
    dependencies :=
      [
        github "anoma" "juvix-stdlib" "v0.10.1";
        github "anoma" "anoma-applib" "v0.7.2";
        github "anoma" "juvix-mtl" "v0.2.1";
      ];
  };
```
{% endcode %}

Let's start with some Juvix code. We will first define an Anoma resource.
