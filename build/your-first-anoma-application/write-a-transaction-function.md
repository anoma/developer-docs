---
description: >-
  This page walks you through writing a transaction function that initializes
  our resource object.
---

# Write a Transaction Function

If you've followed the[ Define a Resource](define-a-resource.md) tutorial, you will have the basic building blocks to write a [transaction function](../../learn/applications/interface.md#transaction-functions). Visit the "LEARN" section to read up on [applications](../../learn/applications/) and the different interface function types.

We're adding to the `HelloWorld.juvix` file. Here, we will write two functions which will ultimately initialize our resource and build the transaction necessary to create the resource. We first write a function `mkHelloWorldTransaction` which passes back an object of type `Transaction`. We then create a `main` function which returns a `TransactionRequest` object.

Alright, let's start with preparing the `Transaction` object:

<pre class="language-agda" data-title="HelloWorld.juvix" data-line-numbers><code class="lang-agda">-- ### Module and previous imports ###

-- ### Helper functions and resource object ###

--- Produces a ;Transaction; that creates a HelloWorld ;Resource;
--- @param nonce A number used to ensure ;Resource; uniqueness.
--- @param message The message to store in the ;Resource;
helloWorldTransaction
  {M : Type -> Type}
  {{Monad M}}
  {{Tx M}} 
  (nonce : Nat) 
  (label : String) 
  : M Transaction :=
  let
<strong>    newResource := mkHelloWorldResource nonce label;
</strong>  in prepareStandardTransaction@{
       -- A Transaction must be balanced, so we consume an ephemeral resource of
       -- the same kind as the one we're creating.
       consumed := [newResource@Resource{ephemeral := true}];
       created := [newResource];
     };
</code></pre>

Let's unpack our `helloWorldTransaction` function. First and foremost, the function is supposed to produce an object of type `Transaction`. takes two arguments, `nonce` and `label` which are passed to the function call of `mkHelloWorldResource`. This allows us to construct a function as shown in [define-a-resource.md](define-a-resource.md "mention") - we pass a `nonce` to ensure the uniqueness of our resource and the `label` to add an arbitrary message, like "Hello World!".

Now, we break new territory. In line 17, we call `prepareStandardTransaction` which is a helpful Applib function to simplify passing back the `Transaction` object. As a reminder, the `Transaction` object is what we expect in line 14 as the return type of the overall `mkHelloWorldTransaction` function.&#x20;

Back to `prepareStandardTransaction` - we call this function by specifying the two arguments `consumed` and `created`. Those arguments represent two key concept that are at the core of Anoma's magic. In short, an Anoma transaction needs to be balanced, i.e. a resource must be consumed so that another resource can be created. Here, we want to create a **non-ephemeral** resource, our `newResource` resource, so we want to consume an **ephemeral** resource of the same specification, another `newResource`. You can learn more about why this is necessary under [transactions](../../learn/transactions/ "mention").

{% hint style="info" %}
The Applib library hides away most of the complexities of building resources and transactions using Anoma. You can still introspect what's happening by looking at the function definiton (in VSCode, use e.g. F12 to maneuver there directly).
{% endhint %}

{% code title="HelloWorld.juvix" %}
```agda
-- ### Previous code for modules, imports, helpers and the resource ###

ctx : TxContext :=
  mkTxContext@{
    caller := Universal.identity;
    currentRoot := mkRoot 0;
  };
```
{% endcode %}

This snippet of code, for now, just assigns dummy values to two important parameters. The `ctx` function takes `caller` which is an identity and `currentRoot` which is the current state root of the commitment tree. For now, it's sufficient to remember that the identity can be used to assign ownable resources and to sign messages. The commitment tree root can proof the valid existence of the resource. In this specific case, we assign `Universal.Identity` which is a universally known public key (derived from a zero address `0x0` seed / private key) and create a dummy state root from `0`.

Now, we add the `main` function:

{% code title="HelloWorld.juvix" %}
```agda
-- Previous code for modules, imports, helpers, 
-- the resource, and the context

--- The function that is run to produce a Transaction to send to Anoma.
main : TransactionRequest :=
  TransactionRequest.fromTransaction (mkHelloWorldTransaction 0 "Hello World!\n");
```
{% endcode %}

The `main` function here serves a straightforward job - it creates a `TransactionRequest` object by called `TransactionRequest.fromTransaction` with the previously written `mkHelloWorldTransaction` function as well as the `nonce` and, finally, the cleartext label "Hello World!".

The completed code of our `HelloWorld.juvix` file should look like the following:

<details>

<summary>See the complete <code>HelloWorld.juvix</code> file.</summary>

{% code title="HelloWorld.juvix" %}
```agda
module HelloWorld;

import Stdlib.Prelude open;
import Applib open;

--- A logic function that is always valid.
logic (publicInputs : Instance) (privateInputs : Witness) : Bool := true;

--- Creates a new ;Resource; that stores a ;String; message.
--- @param nonce A number used to ensure resource uniqueness
--- @param message The message to store in the ;Resource;.
mkHelloWorldResource
  (nonce : Nat)
  (message : String)
  {ephemeral : Bool := false}
  : Resource :=
  mkResource@{
    label := builtinAnomaEncode message;
    logic;
    value := 0;
    quantity := 1;
    nonce;
    ephemeral;
    randSeed := 0;
    nullifierKeyCommitment := 0;
  };

--- Produces a ;Transaction; that creates a HelloWorld ;Resource;
--- @param nonce A number used to ensure ;Resource; uniqueness.
--- @param message The message to store in the ;Resource;
helloWorldTransaction
  {M : Type -> Type} -- polymorphic function with type parameter M
  {{Monad M}} -- additional information for type parameter M
  {{Tx M}} -- random number generator needs side effects / Monad
  (nonce : Nat) 
  (label : String) 
  : M Transaction :=
  let
    newResource := mkHelloWorldResource nonce label;
  in prepareStandardTransaction@{
       -- A Transaction must be balanced, so we consume an ephemeral resource of
       -- the same kind as the one we're creating.
       consumed := [newResource@Resource{ephemeral := true}];
       created := [newResource];
     };

ctx : TxContext :=
  mkTxContext@{
    caller := Universal.identity;
    currentRoot := mkRoot 0;
  };

--- The function that is run to produce a Transaction to send to Anoma.
main : TransactionRequest :=
  buildTransactionRequest 0 ctx (helloWorldTransaction 0 "Hello World!\n");
```
{% endcode %}

</details>

In the following chapter, we will add a projection function to access our resource label.
