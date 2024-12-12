---
icon: rocket-launch
description: >-
  The following sets you up to build, run, and test your first Anoma
  application.
---

# Your First Anoma Application

## Prerequisites

Before creating your first Anoma dApp, make sure to have [Juvix installed](../../further-resources/juvix/install-juvix.md).

Additionally, you can learn how [write a Juvix project](../../further-resources/juvix/write-a-juvix-project.md) to nail the very basics of coding in Juvix.

## Building the HelloWorld Application

### Project Outline

The goal of this application is to create 1) _an Anoma Resource_ with 2) _a `label` "HelloWorld!"_ and two interface functions, 3) _a function to read_ _the_ `label` from the Resource Object as well as 4) _a 'write' function_ to create transactions.

### Project Setup

Let's fly through the project setup by typing the following in our console / terminal. If you already have a project directory, you can skip ahead and just use `juvix init`.

{% code title="~/" %}
```bash
mkdir HelloWorld
cd HelloWorld
juvix init
# (1) Next, type 'hello-world' for the project name
# (2) Finally, press 'Enter' to choose the default version number
```
{% endcode %}
