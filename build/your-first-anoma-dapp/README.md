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

A Juvix project is a collection of Juvix modules and a `Package.juvix` file. You can find more detailed information on `Package.juvix` under [Write a Juvix Package](write-a-juvix-package.md).

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

## Compiling Juvix Project

To compile `HelloWorld.juvix`, go to your terminal and type:

```bash
juvix compile native HelloWorld.juvix
```

This should result in the following project directory structure:

```
hello-world
|- HelloWorld        -- compiled Unix Executable file
|- HelloWorld.juvix
|- Package.juvix
```

## Running Juvix Project

Finally, we can run the compile Unix Executable file by typing the following in our terminal:

```bash
juvix eval HelloWorld.juvix
```

This should now result in the expected output `"Hello world!"`.
