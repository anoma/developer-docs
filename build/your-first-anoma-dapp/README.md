---
icon: rocket-launch
description: The following sets you up to build, run, and test your first Anoma dApp.
---

# Your First Anoma dApp

## Prerequisites

Before creating your first Anoma dApp, make sure to have [Juvix installed](../getting-started/install-juvix.md).

## Anoma Client

<mark style="background-color:orange;">**\[Section about connecting to Client]**</mark>

## Setup a Juvix Project

A Juvix project is a collection of Juvix modules and a `Package.juvix` file. You can find more detailed information on `Package.juvix` under [Write a Juvix Package](configure-a-juvix-package.md).

Open your terminal and navigate to your project directory. Initialize your first project by running:

```bash
juvix init
```

You will be prompted to enter the name of your project, let's go with `hello-world` and a version of your project, let's take the default `0.0.0` by pressing _Enter_.

## Adding a Juvix Module

To add functionality to our project, we need a new Juvix file. A Juvix file must declare a module with the same name as you gave the file. Let's create a new file in our terminal:

```bash
touch HelloWorld.juvix
```

We will first add the module declaration according to the name of the file we have just created.

{% code title="HelloWorld.juvix" %}
```agda
-- This is a comment
module HelloWorld;
```
{% endcode %}



<mark style="background-color:orange;">\[WIP HELLO WORLD RESOURCE OBJECT]</mark>
