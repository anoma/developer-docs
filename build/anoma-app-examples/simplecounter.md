---
description: Learn how to build the SimpleCounter example application.
hidden: true
---

# SimpleCounter

## Goal of this example

The following outlines the basics of the SimpleCounter example application. The focus of this example is to highlight how resources can work with specific values and how additional functionality can access those resource values.

## Prerequisites

Before creating the SimpleCounter application, make sure to have [Juvix installed](../../further-resources/juvix/install-juvix.md). Also consider looking at [Your First Anoma dApp](../your-first-anoma-application/) for some introductory explanations.

## Let's write some SimpleCounter code

#### Project setup

Let's fly through the project setup by typing the following in our console / terminal. If you already have a project directory, you can skip ahead and just use `juvix init`.

```bash
mkdir SimpleCounter
cd SimpleCounter
juvix init
# (1) Type 'simple-counter' for the project name
# (2) Press 'Enter' to choose the default version number
```

This allows us to get started with the first building block of our counter.

#### SimpleCounter Resource

We first build the Resource Object (<mark style="background-color:orange;">link to Resource learn section</mark>) as it is at the very core of our Anoma dApp. We will continue in five steps - we begin with the Resource Object function, then <mark style="background-color:purple;">the Logic, followed by Label, then Value, and lastly we're tying it back together in the Resource Object.</mark>

{% stepper %}
{% step %}
#### Building the Resource Object

We start from the terminal and create a new `Resource` Juvix file.

```bash
touch Resource.juvix
```

We start by sketching out the Resource Object function which we call `mkCounter`.

{% code title="Resource.juvix" lineNumbers="true" %}
```agda
module Resource;

-- Imports section
import Stdlib.Prelude open;
import Anoma open;
import Anoma.Builtin.System open; -- Import for `Reference.to`
import Applib open;

-- Resource Object
mkCounter {count : Nat := 0} {ephemeral : Bool := false} : Resource :=
  mkResource@{
    logicRef := TODO;
    labelRef := TODO;
    valueRef := TODO;
    quantity := 1;
    ephemeral;
    nullifierKeyCommitment := toNullifierKeyCommitment Universal.identity;
    nonce := mkNonce rand;
    randSeed := mkRandSeed rand; 
  };
```
{% endcode %}

Importantly, `TODO` will lead to the IDE complaining `Symbol not in scope: TODO` as it's not defined. We can avoid this error by temporarily defining `TODO` before replacing it step-by-step over the next sections. Thus, we can add above `mkCounter`:

```agda
axiom TODO : {A : Type} -> A;
```

Let's unpack what we just added in the Resource Object section line-by-line.

```agda
mkCounter {count : Nat := 0} {ephemeral : Bool := false} : Resource :=
```

We create a function `mkCounter` which takes in two arguments, `count` and `ephemeral`. `count` is of type `Nat` and has default value `0`, `ephemeral` is of type `Bool` and is `false` by default. Our `mkCounter` function passes back an object of type `Resource`.

Next, we specify the constructor's arguments, please find more info on the `Resource` specification \[<mark style="background-color:orange;">here</mark>].

```agda
mkResource@{...};
```

First, we use the constructor of type `Resource` to instantiate the result of our `mkCounter` function. Now, we assign the individual arguments.

{% code title="Resource.juvix" %}
```agda
mkCounter {count : Nat := 0} {ephemeral : Bool := false} : Resource :=
    mkResource@{
        logicRef := TODO;             -- Will be added in next section
        labelRef := TODO;             -- Will be added in next section
        valueRef := TODO;             -- Will be added in next section
        quantity := 1;                -- We create one counter
        ephemeral;                    -- We pass the mkCounter function argument
        nullifierKeyCommitment := toNullifierKeyCommitment Universal.identity; -- To simplify, we use a univeral non-unique identifier
        nonce := mkNonce rand;        -- We assign a random nonce
        randSeed := mkRandSeed rand;  -- We assign a random seed
    };
```
{% endcode %}

{% hint style="info" %}
You can learn more about how to use the `nullifierKeyCommitment` in a more sophisticated scenario by looking at the `UniqueCounter` example in \[<mark style="background-color:orange;">INSERT ANOMA-APPLIB UNIQUE COUNTER LINK</mark>].
{% endhint %}

Let's now get to the meat of it and specify our Resource further - we start with a simpel label.
{% endstep %}

{% step %}
#### Resource Label

We start by creating a new file that will contain our Resource Label.

{% code title="Project root" %}
```bash
touch Label.juvix
```
{% endcode %}

We add the following code in our new Label file.

{% code title="Label.juvix" lineNumbers="true" %}
```agda
module Label;

import Anoma open;
import Anoma.Builtin.System open;

counterLabel : Label := mkLabel (anomaEncode ("SimpleCounter"))
```
{% endcode %}

The above simply creates a variable `counterLabel` of type `Label` which is assigned to the result of 1) running the `anomaEncode` function on "SimpleCounter" and 2) running `mkLabel` on the result of 1).

{% hint style="info" %}
Throughout the documentation, we're trying to limit the ways we write Juvix code to a right-to-left evaluation. There are other ways though. For instance, instead of writing `mkLabel (anomaEncode ("SimpleCounter"))`, you can write `"SimpleCounter" |> anomaEncode |> mkLabel`, which results in the same but is written left-to-right.
{% endhint %}
{% endstep %}

{% step %}
#### Resource Logic

Let's now write the most complex part of the Resource, the Resource Logic.

{% hint style="info" %}
We're actively working on abstractions to reduce Logic boilerplate code.
{% endhint %}

First and foremost, we can think about the code we're writing as logic that needs to evaluate to true in the case of a balanced transaction. What does that mean? It means our point-of-view is from the Anoma Resource Machine on a transaction with two resources, one consumed and one created resource. Now, in order for this transaction to balance, aka go through, we need both consumed and created resources' Resource Logics to evaluate true. Thus, when writing the following code, assume that the Anoma Resource Machine runs these logics and there are two resources present that the Anoma Resource Machine knows about.

We start by creating the `Logic.juvix` file and adding the standard `module` and `import` code, as well as a dummy `counterLogic` function with two inputs, `publicInputs` and `privateInputs`.

{% code title="Logic.juvix" lineNumbers="true" %}
```agda
module Logic;

-- Imports
import Stdlib.Prelude open;
import Anoma open;
import Applib open;

-- Helper
axiom TODO : {A : Type} -> A;

-- Resource Logic
counterLogic
  (publicInputs : Logic.Instance) (privateInputs : Logic.Witness) : Bool :=
  TODO;

```
{% endcode %}

Here, `publicInputs` include `tag`, `commitments`, `nullifiers`, and `appData` and `privateInputs` include important information like the `Set` of consumed and created Resources.

Our plan for the logic is now to evaluate to true in the two distinct cases that

* 1\) the consumed resource is `Ephemeral` and the created resource is `NonEphemeral` which means that the counter is now getting created (i.e., it has not existed before)
* 2\) the consumed resource is `NonEphemeral` and the created resource is `NonEphemeral` which means that the counter exists and is now getting incremented.

In all other cases, i.e. `NonEphemeral, Ephemeral` and `Ephemeral, Ephemeral`, we want the logic to evaluate `false`.

Let's try to build that with the following pattern of two `case` evaluations.

{% code title="" lineNumbers="true" %}
```agda
module Logic;

-- Imports
import Stdlib.Prelude open;
import Stdlib.Data.Set as Set open using {Set};
import Anoma open;
import Applib open;

-- Resource Logic
counterLogic
  (publicInputs : Logic.Instance) (privateInputs : Logic.Witness) : Bool :=
let
    consumed := privateInputs |> Logic.Witness.consumed |> Set.toList;
    created := privateInputs |> Logic.Witness.created |> Set.toList;
  in case consumed, created of
       | [consumedCounter], [createdCounter] :=
         sameKindCheck@{
           consumedCounter;
           createdCounter;
         }
           && case
                both HasEphemerality.get (consumedCounter, createdCounter)
              of {
                | Ephemeral, NonEphemeral :=
                  creationCheck@{
                    createdCounter;
                  }
                | NonEphemeral, NonEphemeral :=
                  incrementationCheck@{
                    consumedCounter;
                    createdCounter;
                  }
                | _, _ := false
              }
       | _, _ := false;

sameKindCheck (consumedCounter createdCounter : Resource) : Bool :=
  kind consumedCounter == kind createdCounter;

creationCheck (createdCounter : Resource) : Bool :=
  getCount createdCounter == 0 && HasQuantity.get createdCounter == 1;

incrementationCheck (consumedCounter createdCounter : Resource) : Bool :=
  let
    expected := getCount consumedCounter + 1;
    actual := getCount createdCounter;
  in expected == actual;
```
{% endcode %}

<mark style="background-color:orange;">\[BREAK DOWN IN MULTIPLE STEPS]</mark>

<mark style="background-color:orange;">\[^ PASTE NEW CODE ABOVE]</mark>

In order to have this compile, we need to code the `getCount` function first.
{% endstep %}

{% step %}
#### Projection function

We should be able to do this quickly after everything we've learned so far. `getCount` is a special type of function, a projection function. You can think of it as a read function and it lives within the interface \[<mark style="background-color:orange;">LINK TO MODEL VIEW CONTROLLER VISUAL</mark>]. Thus, we create a separate folder, "Interface", and add our new projection function file in there.

```bash
mkdir Interface
cd Interface
touch Projection.juvix
```

Let's create the `getCount` function.

{% code title="Projection.juvix" lineNumbers="true" %}
```agda
module Interface.Projection;

import Stdlib.Prelude open;
import Anoma open;
import Anoma.Builtin.System open;

getCount (resource : Resource) : Nat :=
  Value.unValue (Reference.from (Resource.valueRef (resource)));
```
{% endcode %}

Essentially, `getCount` does exactly what you think it does, it accesses a resource which we pass into the function and reads the value. In our application and for resources we target here, the value field corresponds to the count.

In order to make `Logic.juvix` work, let's not forget to import `getCount`:

```agda
import Interface.Projection open using {getCount};
```

Let's briefly circle back to `Resource.juvix` to finish our Resource Object.
{% endstep %}

{% step %}
#### Complete the Resource Object

We can now pass the missing Resource Object parameters by importing what we've been coding in the above steps.

```agda
module Resource;

-- Imports
import Stdlib.Prelude open;
import Anoma open;
import Anoma.Builtin.System open;
import Applib open;

import Logic open using {counterLogic};
import Label open using {counterLabel};

-- Resource Object
mkCounter {count : Nat := 0} {ephemeral : Bool := false} : Resource :=
  mkResource@{
    logicRef := Reference.to (counterLogic);      -- counterLogic from Logic.juvix
    labelRef := Reference.to (counterLabel);      -- counterLabel from Label.juvix
    valueRef := Reference.to (mkValue (count));   -- count from function arguments
    quantity := 1;
    ephemeral;
    nullifierKeyCommitment := toNullifierKeyCommitment Universal.identity;
    nonce := mkNonce rand;
    randSeed := mkRandSeed rand;
  };
```

In order to pass the correct type, we use the `Reference.to` function. Our `mkCounter` resource now uses `counterLogic`, `counterLabel`, and `count`. As you've correctly noticed, we could've specified `valueRef` in Step 1 as the `count` argument was already present. However, we decided to build up some of the Juvix knowledge, like why we write argument passing from right-to-left, and now put it all together.

We can now face the final boss of our `SimpleCounter` Anoma dApp, the transaction function. Besides the projection function we've already tackled, this is another special function that is part of the `Interface`. As the name suggests, the transaction function handles transactions.

Let's dive into creating our `SimpleCounter` transaction function!
{% endstep %}

{% step %}
#### Transaction function

We first create `Transaction.juvix` inside our `Interface` directory (on the same level as `Projection.juvix`).
{% endstep %}
{% endstepper %}

## Target structure

The project we're building will have the following structure which we will start to build in the following:

```
counter
|- Interface
|-- Projection.juvix
|-- Transaction.juvix
|- Kind.juvix
|- Label.juvix
|- Logic.juvix
|- Resource.juvix
```
