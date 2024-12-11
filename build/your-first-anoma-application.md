---
icon: rocket-launch
description: >-
  The following sets you up to build, run, and test your first Anoma
  application.
---

# Your First Anoma Application

## Prerequisites

Before creating your first Anoma dApp, make sure to have [Juvix installed](../further-resources/juvix/install-juvix.md).

Additionally, you can learn how [write a Juvix project](../further-resources/juvix/write-a-juvix-project.md) to nail the very basics of coding in Juvix.

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

{% stepper %}
{% step %}
### Building the HelloWorld Resource Object

Let's start by creating a new file that will contain our Resource Object.

{% code title="~/HelloWorld" %}
```bash
touch Resource.juvix
```
{% endcode %}

Next, we preface our `Resource.juvix` file with the standard `module` and imports.

{% code title="Resource.juvix" %}
```agda
-- At the start of a Juvix file, we must declare a module with the filename
-- If HelloWorld is within another Juvix project, we'd use `module HelloWorld.Resource`
module Resource;

-- We import the standard library prelude
import Stdlib.Prelude open;

-- We import Anoma libraries to access core functionality like `anomaEncode`
import Anoma open;
import Anoma.Builtin.System open;
```
{% endcode %}

We now start contructing our Resource which we call `mkHelloWorldResource` by giving it values for `logic`, `label`, `value`, `quantity`, `ephemeral`, `nonce`, `randSeed` and `nullifierKeyCommitment` .

Let's start by putting default values for `logic` and `value` while assigning `TODO` to `label` for now.

{% code title="Resource.juvix" lineNumbers="true" %}
```agda
--- ### Module and imports ###

--- logic assigns a certain logic to the Resource that is checked when it's consumed
---
--- @param publicInputs takes all public inputs, i.e. commitments, nullifiers, a tag, and appData
--- @param privateInputs takes all private inputs, i.e. list of created, consumed resources and customInputs
---
--- Returns a Boolean
logic (publicInputs : Logic.Instance) (privateInputs : Logic.Witness) : Bool :=
  true;

--- value gives the Resource a programmable value
---
--- Returns a Nat
value : Nat := 0;

--- mkHelloWorldResource constructs a Resource Object
---
--- @param nonce to avoid duplicate resources
--- @param ephemeral to indicate the resource's ephemerality, by default `false`
---
--- Returns a Resource
mkHelloWorldResource (nonce : Nonce) {ephemeral : Bool := false} : Resource :=
  mkResource@{
    label := TODO;
    logic;
    value;
  };
```
{% endcode %}

{% hint style="info" %}
Juvix will complain about the presence of `TODO` in the above code. You can get rid of the errors by adding `axiom TODO : {A : Type} -> A;` above the Resource Object. Keep in mind that you have to remove the `axiom` before compiling the code as it won't compile otherwise.
{% endhint %}

What's happening here is that we assign `true` to the `logic`, implying that this Resource will not have any logic constraints. Similarly, we assign zero for `value` which is an arbitrary default.

Let's continue adding missing parameters before getting back to `label`.

{% code title="Resource.juvix" lineNumbers="true" %}
```agda
-- ### Module, imports, and logic / value helper functions ###

mkHelloWorldResource (nonce : Nonce) {ephemeral : Bool := false} : Resource :=
  mkResource@{
    label := TODO;
    logic;
    value;
    quantity := 1;
    nonce := nonceToNat nonce;
    ephemeral;
    randSeed := 0;
    nullifierKeyCommitment := 0;
  };
```
{% endcode %}

In the above code, we

* assign `quantity` of 1 (line 8)
* pass function parameter `nonce`(line 9)
* pass function parameter `ephemeral`(line 10)
* assign default parameter 0 to `randSeed`(line 11)
* assign default parameter 0 to `nullifierKeyCommitment`(line 12)

{% hint style="info" %}
**These default values are artifacts of the current devnet implementation.**
{% endhint %}
{% endstep %}

{% step %}
### Add HelloWorld Label

Let's now add our custom label in `Resource.juvix`.&#x20;

{% code title="Resource.juvix" %}
```agda
-- ### Module, imports, and logic / value helper functions ###

label : Nat := anomaEncode "Hello World!";

mkHelloWorldResource (nonce : Nonce) {ephemeral : Bool := false} : Resource :=
  mkResource@{
    label; -- Here we can now just add our label function from above
    logic;
    value;
    quantity := 1;
    nonce := nonceToNat nonce;
    ephemeral;
    randSeed := 0;
    nullifierKeyCommitment := 0;
  };
```
{% endcode %}

At this point, our `Resource.juvix` file is complete and should look like the following.

{% code title="Resource.juvix" %}
```agda
-- At the start of a Juvix file, we must declare a module with the filename
module HelloWorld.Resource;

-- We import the standard library prelude
import Stdlib.Prelude open;

-- We import Anoma libraries to access core functionality like `anomaEncode`
import Anoma open;
import Anoma.Builtin.System open;

--- logic assigns a certain logic to the Resource that is checked when it's consumed
---
--- @param publicInputs takes all public inputs, i.e. commitments, nullifiers, a tag, and appData
--- @param privateInputs takes all private inputs, i.e. list of created, consumed resources and customInputs
--- 
--- Returns a Boolean
logic (publicInputs : Logic.Instance) (privateInputs : Logic.Witness) : Bool :=
  true;

--- value gives the Resource a programmable value
---
--- Returns a Nat
value : Nat := 0;

--- label takes a string and applies anomaEncode to it
---
--- Returns the Nat "Hello world!".
label : Nat := anomaEncode "Hello world!";

--- mkHelloWorldResource constructs a Resource Object
---
--- @param nonce to avoid duplicate resources
--- @param ephemeral to indicate the resource's ephemerality, by default `false`
---
--- Returns a Resource
mkHelloWorldResource (nonce : Nonce) {ephemeral : Bool := false} : Resource :=
  mkResource@{
    label;
    logic;
    value;
    quantity := 1;
    nonce := nonceToNat nonce;
    ephemeral;
    randSeed := 0;
    nullifierKeyCommitment := 0;
  };
```
{% endcode %}
{% endstep %}

{% step %}
### Add HelloWorld Projection Function

Let's enrich our HelloWorld application by adding functionality that allows us to read the label we have just created.

To achieve this, we add `getLabel`, which is a special type of function, a projection function. You can think of it as a read function and it lives within the interface \[<mark style="background-color:orange;">LINK TO MODEL VIEW CONTROLLER VISUAL</mark>]. Thus, we create a separate folder, "Interface", and add our new projection function file in there.

{% code title="~/HelloWorld" %}
```bash
mkdir Interface
cd Interface/
touch Projection.juvix
```
{% endcode %}

We can now specify `getLabel`. It will take a parameter of type `Resource` and then access its label via `label`. We finally retrieve the underlying `String` type by applying `anomaDecode` on the `label` of our Resource.

{% code title="Interface/Projection.juvix" %}
```agda
module Interface.Projection;

import Stdlib.Prelude open;
import Anoma open;

--- getLabel retrieves the label from a given Resource
---
--- @param resource to extract the label from
---
--- Returns a String
getLabel (resource : Resource) : String :=
   anomaDecode (Resource.label resource);
```
{% endcode %}
{% endstep %}

{% step %}
### Add HelloWorld Transaction Function

Similar as for our projection function, we're creating a second file in our `Interface` folder which we're calling `Transaction.juvix`.

Here, we will write functions which will ultimately initialize our Resource and build the transaction necessary to create the Resource. Essentially, this is achieve in 5 consecutive steps. We first write a function `prepareTransaction` which passes back an object of type `Transaction`. We then write an `initialize` function which makes use of the prepared transaction object. Next, we construct our standard inputs and stitch together the standard inputs and initialize function as a fourth step. Finally, we create a `main` function which return a `TransactionRequest` object.

Alright, let's start with preparing the `Transaction` object:

{% code title="Interface/Transaction.juvix" %}
```agda
module HelloWorld.Interface.Transaction;

import Anoma open;
import Applib open;
import Stdlib.Prelude open;

import Anoma.State.CommitmentTree open;
import HelloWorld.Resource open;
import BaseLayer.TransactionRequest open;

--- prepareTransaction takes two lists of Resources and turns them into a Transaction object
---
--- @param consumed list of resources that should be consumed in this tx
--- @param created list of resources that should be created in this tx
---
--- Returns object of type Transaction
prepareTransaction (consumed created : List Resource) : Transaction :=
  mkTransactionHelper@{
    actions :=
      [
        mkActionHelper@{
          consumed;
          created;
        };
      ];
  };
```
{% endcode %}

Our `prepareTransaction` function now takes two lists of resources, one for consumed and one for created resources, and returns an object of type `Transaction`. In the process, we use the `mkTransactionHelper` which hides some complexity while we can just pass it the actions that are involved in the transaction, here just consumed and created resources.

{% hint style="info" %}
The Applib library hides away some of the complexities of building Resources and Transactions using Anoma. You can still introspect what's happening by looking at the function definiton (in VSCode, use e.g. F12 to maneuver there directly).
{% endhint %}

Next, we write the `initialize` function:

{% code title="Interface/Transaction.juvix" lineNumbers="true" %}
```agda
--- initialize injects nonces into the resources of a previously prepared transaction object
---
--- @param standardInputs of type StandardInputs consist of caller, currentRoot, and randSeed
---
--- Returns object of type Transaction
initialize (standardInputs : StandardInputs) : Transaction :=
  let
    (nonce1, nonce2) :=
      generateNoncePair (StandardInputs.randSeed standardInputs);
  in prepareTransaction@{
       consumed :=
         [
           mkHelloWorldResource@{
             nonce := nonce1;
             ephemeral := true;
           };
         ];
       created :=
         [
           mkHelloWorldResource@{
             nonce := nonce2;
             ephemeral := false;
           };
         ];
     };
```
{% endcode %}

On a high level, this function takes a parameter `standardInputs` , computes nonces using the `randSeed` of provided `standardInputs` , and then injects those nonces into the two HelloWorld resources which we, in turn, pass into the `prepareTransaction` function to receive back a `Transaction` object.

Note, that there is a distinct difference between the consumed and create HelloWorld resources. In line 15, we specify the consumed resource to be ephemeral (for a rough mental model, that's a short-lived resource). However, in line 22, we specify that the created HelloWorld resource is non-ephemeral, which in this context makes sense as we want this created resource to exist (somewhat) permanently.

This is by far the most complex piece of code in this tutorial, so please take your time to understand how the transaction is initialized and potentially even what happens under the hood in the Applib library.

Let's continue with the standard inputs which we will pass into our `initialize` function in the next step.

{% code title="Interface/Transaction.juvix" %}
```agda
std : StandardInputs :=
  mkStandardInputs@{
    caller := Universal.identity;
    currentRoot := mkRoot 0;
    randSeed := 0;
  };
```
{% endcode %}

Please mind that all these values are default values. The `Universal.Indentity` is an identity generated from the 0 address and as you can tell, both `currentRoot` and `randSeed` are 0 as well.

{% hint style="info" %}
**These default values are artifacts of the current devnet implementation.**
{% endhint %}

From now, it's pretty straightforward. We pass the standard inputs which we just defined into the `initialize` function from before.

{% code title="Interface/Transaction.juvix" %}
```agda
tx : Transaction := initialize std;
```
{% endcode %}

And now we can finish the transaction file with the `main` function which creates the resulting `TransactionRequest` object from our constructed transaction.

{% code title="Interface/Transaction.juvix" %}
```agda
main : TransactionRequest := TransactionRequest.fromTransaction tx; 
```
{% endcode %}

The completed code of our `Transaction.juvix` file should look like the following:

{% code title="Interface/Transaction.juvix" %}
```agda
module HelloWorld.Interface.Transaction;

import Anoma open;
import Applib open;
import Stdlib.Prelude open;

import Anoma.State.CommitmentTree open;
import HelloWorld.Resource open;
import BaseLayer.TransactionRequest open;

initialize (standardInputs : StandardInputs) : Transaction :=
  let
    (nonce1, nonce2) :=
      generateNoncePair (StandardInputs.randSeed standardInputs);
  in prepareTransaction@{
       consumed :=
         [
           mkHelloWorldResource@{
             nonce := nonce1;
             ephemeral := true;
           };
         ];
       created :=
         [
           mkHelloWorldResource@{
             nonce := nonce2;
             ephemeral := false;
           };
         ];
     };

prepareTransaction (consumed created : List Resource) : Transaction :=
  mkTransactionHelper@{
    actions :=
      [
        mkActionHelper@{
          consumed;
          created;
        };
      ];
  };

std : StandardInputs :=
  mkStandardInputs@{
    caller := Universal.identity;
    currentRoot := mkRoot 0;
    randSeed := 0;
  };

tx : Transaction := initialize std;

main : TransactionRequest := TransactionRequest.fromTransaction tx;
```
{% endcode %}
{% endstep %}
{% endstepper %}

## Runing the HelloWorld Application

Amazing, we have written the code we need to compile a HelloWorld Resource Object, create a transaction to initialize our HelloWorld resource, and finally we can read its label via our projection function. Let's run through these steps by starting with the setup of the local Anoma Client and Node.

### Anoma Client and Node

{% hint style="info" %}
The following way of running the Anoma Client and Node are temporary and thus, Work-In-Progress areas which are specific to the **current stage of devnet**.
{% endhint %}

First and foremost, we need a local version of the `testnet-01` branch of [Anoma's codebase](https://github.com/anoma/anoma/tree/testnet-01), install its dependencies, and compile the code. For a detailed description on how to do these steps, please follow this [README](https://github.com/anoma/anoma/blob/testnet-01/README.md).

<mark style="background-color:orange;">TODO: Make sure test</mark> <mark style="background-color:orange;"></mark><mark style="background-color:orange;">`testnet-01`</mark><mark style="background-color:orange;">branch is working by testing on clean device</mark>

Assuming you have successfully compiled the Anoma codebase, you can use the following `makefile` to start the Anoma Client and Anoma Node.

{% file src="../.gitbook/assets/makefile" %}
Makefile to start Anoma Client and Anoma Node, copy in project root
{% endfile %}

<mark style="background-color:orange;">TODO: Change</mark> <mark style="background-color:orange;"></mark><mark style="background-color:orange;">`makefile`</mark><mark style="background-color:orange;">to assume flat folder structure and one command for both client and node</mark>

Open a new terminal in your project root:

{% code title="~/HelloWorld" %}
```bash
make run-client-and-node
```
{% endcode %}

### Calling the Transaction Function

Now, once the Anoma Client and Node have started in your first terminal which you will keep open, we want to call our transaction function. We do so by opening a new terminal and running the following `makefile` command:

{% code title="~/HelloWorld" %}
```bash
make add-transaction
```
{% endcode %}

The above command will trigger our code to compile and get proved.

### Calling the Projection Function

The last step of what we would like to do with our HelloWorld resource is retrieve its label by calling the projection function.

<mark style="background-color:orange;">TODO: Add call to projection function</mark>

