---
description: >-
  This chapter walks you through writing a transaction function that initializes
  our resource object.
---

# Write a Transaction Function

If you've followed the[ Define a Resource](define-a-resource.md) tutorial, you will have the basic building blocks to write a [transaction function](../../learn/applications/interface.md#transaction-functions). Visit the "LEARN" section to read up on [applications](../../learn/applications/) and the different interface function types.

We're creating a new file which we're calling `Transaction.juvix`. Here, we will write functions which will ultimately initialize our Resource and build the transaction necessary to create the Resource. Essentially, this is achieve in 5 consecutive steps. We first write a function `prepareTransaction` which passes back an object of type `Transaction`. We then write an `initialize` function which makes use of the prepared transaction object. Next, we construct our standard inputs and stitch together the standard inputs and initialize function as a fourth step. Finally, we create a `main` function which return a `TransactionRequest` object.

Alright, let's start with preparing the `Transaction` object:

{% code title="~/Transaction.juvix" %}
```agda
module Transaction;

import Anoma open;
import Applib open;
import Stdlib.Prelude open;

import Anoma.State.CommitmentTree open;
import Resource open;
import BaseLayer.TransactionRequest open;

--- Prepares a transaction object from lists of consumed and created resoruces.
--- @param consumed The resources to be consumed in this transaction.
--- @param created The resources to be created in this transaction.
--- @return The prepared transaction object.
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

{% code title="Transaction.juvix" lineNumbers="true" %}
```agda
--- Initializes a HelloWorld resource by consuming an ephemeral one.
--- @param standardInputs The standard inputs (caller, currentRoot, and randSeed).
--- @return The transaction object initializing a HelloWorld resource.
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

This function takes a parameter `standardInputs`, computes nonces using the `randSeed` of provided `standardInputs`, and then injects those nonces into the two `HelloWorld` resources which we, in turn, pass into the `prepareTransaction` function to receive back a `Transaction` object.

Note, that there is a distinct difference between the consumed and create `HelloWorld` resources: The consumed resource is [ephemeral](../../learn/resources/#ephemeral-resources) (which means that it exists only over the course of the transaction). The created `HelloWorld` resource is non-ephemeral, which in this context makes sense as we want this resource to be created.

This is the most complex code in this tutorial, so take your time to understand how the `HelloWorld` resource gets initialized. You can also take a look at the `mkTransactionHelper` and `mkActionHelper` to see what is happening under the hood.

Let's continue with the standard inputs which we will pass into our `initialize` function in the next step.

{% code title="Transaction.juvix" %}
```agda
std : StandardInputs :=
  mkStandardInputs@{
    caller := Universal.identity;
    currentRoot := mkRoot 0;
    randSeed := 0;
  };
```
{% endcode %}

Please mind that all these values are default values. The `Universal.Indentity` is an identity generated from the 0 seed and as you can tell, both `currentRoot` and `randSeed` are 0 as well.

{% hint style="warning" %}
These default values are artifacts of the current devnet implementation.
{% endhint %}

From now, it's pretty straightforward. We pass the standard inputs which we just defined into the `initialize` function from before.

{% code title="Transaction.juvix" %}
```agda
tx : Transaction := initialize std;
```
{% endcode %}

And now we can finish the transaction file with the `main` function which creates the resulting `TransactionRequest` object from our constructed transaction.

{% code title="Transaction.juvix" %}
```agda
main : TransactionRequest := TransactionRequest.fromTransaction tx; 
```
{% endcode %}

The completed code of our `Transaction.juvix` file should look like the following:

<details>

<summary>See the complete <code>Transaction.juvix</code> file.</summary>

{% code title="Transaction.juvix" %}
```agda
module Transaction;

import Anoma open;
import Applib open;
import Stdlib.Prelude open;

import Anoma.State.CommitmentTree open;
import Resource open;
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

</details>

In the following chapter, we will compile and "deploy" our code locally.
