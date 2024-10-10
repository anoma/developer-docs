---
description: The following sets you up to build, run, and test your first Anoma dApp.
icon: rocket-launch
---

# Your First Anoma dApp

Before creating your first Anoma dApp, make sure to have [Juvix installed](../getting-started/install-juvix.md).

<mark style="background-color:orange;">**\[Section about connecting to Client]**</mark>

## Setup a Juvix Project

A Juvix project is a collection of Juvix modules and a `Package.juvix` file.

Initialize your first project by running:

```bash
juvix init
```

You will be prompted to enter the name of your project, let's go with `FirstProject` and a version of your project, let's take the default `0.0.0` by pressing _Enter_.

## Adding a Juvix Module

To add functionality to our first project, we need a separate Juvix file. A Juvix file must declare a module with the same name as the file. Let's create a new file:

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

Our `HelloWorld` program is supposed to output "Hello World!", of course. Thus, we first need to import the `String` type from the standard library prelude.

{% code title="HelloWorld.juvix" fullWidth="false" %}
```agda
module HelloWorld;
    
    -- Importing the `String` type from the standard library prelude
    import Stdlib.Prelude open; 
```
{% endcode %}

Now that we have access to `String`, we can add our "Hello World!" output.

{% code title="HelloWorld.juvix" %}
```agda
module HelloWorld;

  import Stdlib.Prelude open;

  main : String := "Hello world!";
```
{% endcode %}



