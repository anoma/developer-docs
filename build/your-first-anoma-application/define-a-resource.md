---
description: >-
  In this chapter, we're defining a resource object and giving it a custom
  label, "Hello World!".
---

# Define a Resource

{% stepper %}
{% step %}
### Defining the Resource Object

Let's start by creating a new file that will contain our resource definition.

{% code title="~/HelloWorld" %}
```bash
touch Resource.juvix
```
{% endcode %}

Next, we preface our `Resource.juvix` file with the standard `module` and `import`s.

{% code title="Resource.juvix" overflow="wrap" %}
```agda
-- At the start of a Juvix file, we must declare a module with the filename.
module Resource;

-- We import the standard library prelude.
import Stdlib.Prelude open;

-- We import Anoma libraries to access core functionality like `anomaEncode`.
import Anoma open;
import Anoma.Builtin.System open;
```
{% endcode %}

We now start defining our resource which we call `mkHelloWorldResource` by giving it values for `logic`, `label`, `value`, `quantity`, `ephemeral`, `nonce`, `randSeed` and `nullifierKeyCommitment`.

Let's start by putting default values for `logic` and `value` while assigning `TODO` to `label` for now.

{% code title="Resource.juvix" overflow="wrap" %}
```agda
-- ### Module, imports ###

--- A logic function always returning `true`.
--- @param publicInputs Public inputs to the logic function (not used).
--- @param privateInputs Private inputs to the logic function (not used).
--- @return Whether the logic funtion is valid or not.
logic (publicInputs : Logic.Instance) (privateInputs : Logic.Witness) : Bool := true;

--- Constructs a resource object.
--- @param nonce A nonce ensuring a unique resource commitment hash.
--- @param ephemeral The resource's ephemerality (default: `false`).
--- @return The constructed resource object.
mkHelloWorldResource (nonce : Nonce) {ephemeral : Bool := false} : Resource :=
  mkResource@{
    logic;
    label := TODO; -- We put a TODO here for now.
    value := 0;    -- We don't put any data in here in this example.
  };
```
{% endcode %}

What's happening here is that we assign `true` to the `logic`, implying that this Resource will not have any logic constraints. Similarly, we assign zero for `value` which is an arbitrary default.

Let's continue adding missing parameters before getting back to `label`.

{% code title="Resource.juvix" overflow="wrap" %}
```agda
-- ### Module, imports, and logic ###

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

* assign `quantity` of 1
* pass function parameter `nonce`
* pass function parameter `ephemeral`
* assign default parameter 0 to `randSeed`
* assign default parameter 0 to `nullifierKeyCommitment`

{% hint style="warning" %}
These default values are artifacts of the current devnet implementation.
{% endhint %}
{% endstep %}

{% step %}
### Add the Label

Let's now add our custom label in `Resource.juvix`.&#x20;

{% code title="Resource.juvix" %}
```agda
-- ### Module, imports, and logic / value helper functions ###

label : Nat := anomaEncode "Hello World!";

mkHelloWorldResource (nonce : Nonce) {ephemeral : Bool := false} : Resource :=
  mkResource@{
    logic;
    label; -- We removed the `:= 0` so that the label defined above is used.
    value := 0;
    quantity := 1;
    nonce := nonceToNat nonce;
    ephemeral;
    randSeed := 0;
    nullifierKeyCommitment := 0;
  };
```
{% endcode %}

At this point, our `Resource.juvix` file is complete
{% endstep %}
{% endstepper %}

<details>

<summary>See the complete <code>Resource.juvix</code> file.</summary>

{% code title="Resource.juvix" %}
```agda
module HelloWorld.Resource;

import Stdlib.Prelude open;
import Anoma open;
import Anoma.Builtin.System open;

--- A logic function always returning `true`.
--- @param publicInputs Public inputs to the logic function (not used).
--- @param privateInputs Private inputs to the logic function (not used).
--- @return Whether the logic funtion is valid or not.
logic (publicInputs : Logic.Instance) (privateInputs : Logic.Witness) : Bool :=
  true;

--- label takes a string and applies anomaEncode to it
--- @return object of type Nat
label : Nat := anomaEncode "Hello world!";

--- Constructs a resource object.
--- @param nonce A nonce ensuring a unique resource commitment hash.
--- @param ephemeral The resource's ephemerality (default: `false`).
--- @return Whether the logic funtion is valid or not.
mkHelloWorldResource (nonce : Nonce) {ephemeral : Bool := false} : Resource :=
  mkResource@{
    logic;
    label;
    value := 0;
    quantity := 1;
    nonce := nonceToNat nonce;
    ephemeral;
    randSeed := 0;
    nullifierKeyCommitment := 0;
  };
```
{% endcode %}

</details>

Next, we're going to build the transaction function which will be used to initialize the resource object via transaction we manually prepare with our code.

