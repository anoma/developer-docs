---
description: >-
  In this page, we're defining a resource object and preparing it to receive a
  custom message.
---

# Define a Resource

### Defining the Resource Object

Let's start by creating a new file that will contain our resource definition.

{% code title="~/HelloWorld/" %}
```bash
touch HelloWorld.juvix
```
{% endcode %}

Next, we preface our `HelloWorld.juvix` file with the standard `module` and `import`s.

{% code title="HelloWorld.juvix" overflow="wrap" %}
```agda
-- At the start of a Juvix file, we must declare a module with the filename.
module HelloWorld;

-- We import the standard library prelude as well as the Applib library and a library -- for encoding our logic in the next step.
import Stdlib.Prelude open;
import Applib open;
import Anoma.Encode open;
```
{% endcode %}

We now start defining our resource which we call `mkHelloWorldResource` by giving it values for `logic`, `label`, `value`, `quantity`, `ephemeral`, `nonce`, `randSeed` and `nullifierKeyCommitment`. You can find more detailed information on the resource and its parameters in [resources](../../learn/resources/ "mention").

Let's start by putting default values for `logic` and `value` and pass the `message` parameter to `label`.

{% code title="HelloWorld.juvix" overflow="wrap" %}
```agda
-- ### Module, imports ###

--- A logic function that is always valid.
logic : Logic := Logic.mk \{_ := true};

--- Creates a new ;Resource; that stores a ;String; message.
--- @param nonce A number used to ensure resource uniqueness
--- @param message The message to store in the ;Resource;.
mkHelloWorldResource
  (nonce : Nonce)
  (message : String)
  {ephemeral : Bool := false}
  : Resource :=
  Resource.mk@{
    label := Label.fromNat (builtinAnomaEncode message);
    logic := Encoded.encode logic;
    value := AnomaAtom.fromNat 0;
  };
```
{% endcode %}

In the context of our HelloWorld application, this implies that we're passing the "Hello World!"  `message` when we're creating the resource. This allows us to set any string, like "Hello Anomages!". In this specific application, this might not seem too useful as we're going to hardcode the message as part of the transaction function. Nevertheless, this becomes very helpful when e.g. adding a frontend that allows us to specify an arbitrary message - check out [this repository](https://github.com/anoma/anoma-apps/tree/main/HelloWorld) for inspiration.&#x20;

In addition to that we create an object of type `Logic` which is always `true` and pass it to `logic`, implying that this resource will not have any logic constraints. Similarly, we assign zero for `value` which is an arbitrary default.

Let's continue adding missing parameters:

{% code title="HelloWorld.juvix" overflow="wrap" %}
```agda
-- ### Module, imports, and logic ###

mkHelloWorldResource
  (nonce : Nonce) 
  (message : String) 
  {ephemeral : Bool := false} 
  : Resource :=
  Resource.mk@{
    label := Label.fromNat (builtinAnomaEncode message);
    logic := Encoded.encode logic;
    value := AnomaAtom.fromNat 0;
    quantity := 1;
    nonce := Nonce.toRaw nonce;
    ephemeral;
    unusedRandSeed := 0;
    nullifierKeyCommitment := AnomaAtom.fromNat 0;
  };
```
{% endcode %}

In the above code, we

* assign `quantity` of 1
* pass function parameter `nonce`
* pass function parameter `ephemeral`
* assign default parameter 0 to `unusedRandSeed`
* assign default parameter 0 to `nullifierKeyCommitment`

{% hint style="warning" %}
These default values are artifacts of the current devnet implementation.
{% endhint %}

<details>

<summary>See the complete <code>HelloWorld.juvix</code> file.</summary>

{% code title="HelloWorld.juvix" %}
```agda
module HelloWorld;

import Stdlib.Prelude open;
import Applib open;
import Anoma.Encode open;

--- A logic function that is always valid.
logic : Logic := Logic.mk \{_ := true};

--- Creates a new ;Resource; that stores a ;String; message.
--- @param nonce A number used to ensure resource uniqueness
--- @param message The message to store in the ;Resource;.
mkHelloWorldResource
  (nonce : Nonce)
  (message : String)
  {ephemeral : Bool := false}
  : Resource :=
  Resource.mk@{
    label := Label.fromNat (builtinAnomaEncode message);
    logic := Encoded.encode logic;
    value := AnomaAtom.fromNat 0;
    quantity := 1;
    nonce := Nonce.toRaw nonce;
    ephemeral;
    unusedRandSeed := 0;
    nullifierKeyCommitment := AnomaAtom.fromNat 0;
  };
```
{% endcode %}

</details>

Next, we're going to build the transaction function which will be used to initialize the resource object via transaction we manually prepare with our code.
