---
icon: rocket-launch
description: >-
  The following sets you up to build, run, and test your first Anoma
  application.
---

# Your First Anoma Application

## Prerequisites

Before creating your first Anoma dApp, make sure to have [Juvix installed](../further-resources/advanced/juvix/install-juvix.md).

Additionally, you can learn how [write a Juvix project](../further-resources/advanced/juvix/write-a-juvix-project.md) to nail the very basics of coding in Juvix.

## Anoma Client

<mark style="background-color:orange;">\[Section about connecting to Client]</mark>

## Building the HelloWorld Application

#### Project Outline

The goal of this application is to create 1) _an Anoma Resource_ with 2) _a `label` "HelloWorld!"_ as well as 3) _a function to read_ _the_ `label` from the Resource Object.

<mark style="background-color:orange;">\[could insert more formal explainer of 1) Resource and 2) Label]</mark>

#### Project Setup

Let's fly through the project setup by typing the following in our console / terminal. If you already have a project directory, you can skip ahead and just use `juvix init`.

```bash
mkdir HelloWorld
cd HelloWorld
juvix init
# (1) Next, type 'hello-world' for the project name
# (2) Finally, press 'Enter' to choose the default version number
```

{% stepper %}
{% step %}
#### Building the HelloWorld Resource Object

Let's start by creating a new file that will contain our Resource.

{% code title="# Inside ~/HelloWorld# Inside ~/HelloWorld" %}
```bash
touch Resource.juvix
```
{% endcode %}

Next, we preface our `Resource.juvix` file with the standard `module` and imports.

{% code title="Resource.juvix" %}
```agda
module Resource;

-- IMPORTS
import Stdlib.Prelude open;
import Anoma open;
import Anoma.Builtin.System open;
import Applib open;
```
{% endcode %}

We now start contructing our Resource which we call `mkHelloWorld` by giving it values for `logicRef`, `labelRef`, `valueRef`, `quantity`, `ephemeral`, `nullifierKeyCommitment`, `nonce`, and `randSeed`.

Let's start by putting default values for `logicRef` and `valueRef` while assigning `TODO` to `labelRef` for now. We will then branch out the `labelRef` code into a `Label.juvix` file to emphasize that we're not dealing with default values here.

{% code title="Resource.juvix" lineNumbers="true" %}
```agda
-- ### Module and imports code ###

-- HELPER
logic (publicInputs : Logic.Instance) (privateInputs : Logic.Witness) : Bool :=
  true;

value : Value := mkValue 0;

-- RESOURCE OBJECT
mkHelloWorld : Resource :=
  mkResource@{
    labelRef := TODO;
    logicRef := Reference.to (logic);
    valueRef := Reference.to (value);
  };
```
{% endcode %}

{% hint style="info" %}
Juvix will complain about the presence of `TODO` in the above code. You can get rid of the errors by adding `axiom TODO : {A : Type} -> A;` above the Resource Object. Keep in mind that you have to remove the `axiom` before compiling the code as it won't compile otherwise.
{% endhint %}

What's happening here is that we assign `true` to the `logicRef`, implying that this Resource will not have any logic constraints. Similarly, we assign zero for `valueRef` which is an arbitrary default.

Let's continue adding missing parameters before getting back to `labelRef`.

{% code title="Resource.juvix" lineNumbers="true" %}
```agda
-- ### Module, imports, and helpers code ###

-- RESOURCE OBJECT
mkHelloWorld : Resource :=
  mkResource@{
    labelRef := TODO;
    logicRef := Reference.to (logic);
    valueRef := Reference.to (value);
    quantity := 1;
    ephemeral := false;
    nullifierKeyCommitment := toNullifierKeyCommitment Universal.identity;
    nonce := mkNonce rand;
    randSeed := mkRandSeed rand;
  };
```
{% endcode %}

In the above code, we

* assign `quantity` of 1 (line 9)
* specify a non-emphemeral resource (in a sense, it can persist) (line 10)
* assign the universal identity to `nullifierKeyCommitment` which is equal to an Identity generated from a zero address (line 11)
* use builtin `rand` for our `nonce` (line 12)
* and finally use builtin `rand` again for the `randSeed` (line 13)
{% endstep %}

{% step %}
#### Add HelloWorld Label

Let's add our `label` in a separate file. Although the `label` is not a complex piece of code in this case, it is good practice to have a separate file for non-default Resource components.

{% code title="# Inside ~/HelloWorld# Inside ~/HelloWorld" %}
```bash
touch Label.juvix
```
{% endcode %}

We can now populate the `Label.juvix` file with our `Hello World!` label.

{% code title="Label.juvix" %}
```agda
module Label;

import Anoma open;
import Anoma.Builtin.System open;

label : Label := mkLabel (anomaEncode "Hello World!");
```
{% endcode %}

In the above code, we simply create a variable `label` of type `Label` which is the result of applying the function `mkLabel` to the result of the function `anomaEncode` of `Hello World!`. In other words, we encode "Hello World!" and then turn that into a label.

Last step for our label is to tweak the `Resource.juvix` file slightly to accomodate our new code. The following should be our `Resource.juvix` file after integrating the label.

{% code title="Resource.juvix" %}
```agda
module Resource;

-- IMPORTS
import Stdlib.Prelude open;
import Anoma open;
import Anoma.Builtin.System open;
import Applib open;

import Label open using {label};

-- HELPER
logic (publicInputs : Logic.Instance) (privateInputs : Logic.Witness) : Bool :=
  true;

value : Value := mkValue 0;

-- RESOURCE OBJECT
mkHelloWorld : Resource :=
  mkResource@{
    labelRef := Reference.to (label);
    logicRef := Reference.to (logic);
    valueRef := Reference.to (value);
    quantity := 1;
    ephemeral := false;
    nullifierKeyCommitment := toNullifierKeyCommitment Universal.identity;
    nonce := mkNonce rand;
    randSeed := mkRandSeed rand;
  };

```
{% endcode %}
{% endstep %}

{% step %}
#### Add HelloWorld Projection Function

Let's finish our HelloWorld application by adding functionality that allows us to read the label. To achieve this, we add `getLabel`, which is a special type of function, a projection function. You can think of it as a read function and it lives within the interface \[<mark style="background-color:orange;">LINK TO MODEL VIEW CONTROLLER VISUAL</mark>]. Thus, we create a separate folder, "Interface", and add our new projection function file in there.

{% code title="# Inside ~/HelloWorld" %}
```bash
mkdir Interface
cd Interface/
touch Projection.juvix
```
{% endcode %}

We can now specify `getLabel`. It will take a parameter of type `Resource` and then access its label via `labelRef`.

{% code title="Interface/Projection.juvix" %}
```agda
module Interface.Projection;

import Stdlib.Prelude open;
import Anoma open;
import Anoma.Builtin.System open;

getLabel (resource : Resource) : Nat := Label.unLabel (Reference.from (Resource.labelRef (resource)));
```
{% endcode %}
{% endstep %}
{% endstepper %}

## Runing the HelloWorld Application

<mark style="background-color:orange;">\[Continue section with progress from Paul's e2e integration work]</mark>
