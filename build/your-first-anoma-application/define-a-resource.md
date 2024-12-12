---
description: >-
  In this chapter, we're writing the Resource Object and giving it a custom
  label, "Hello World!".
---

# Define a Resource

{% stepper %}
{% step %}
### Constructing the Resource Object

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
-- ### Module and imports ###

--- logic assigns a certain logic to the Resource that is checked when it's consumed
---
--- @param publicInputs takes all public inputs, i.e. commitments, nullifiers, a tag, and appData
--- @param privateInputs takes all private inputs, i.e. list of created, consumed resources and customInputs
---
--- @return object of type Boolean
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
--- @return object of type Resource
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

{% hint style="warning" %}
These default values are artifacts of the current devnet implementation.
{% endhint %}
{% endstep %}

{% step %}
### Add Custom HelloWorld Label

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
--- @return object of type Boolean
logic (publicInputs : Logic.Instance) (privateInputs : Logic.Witness) : Bool :=
  true;

--- value gives the Resource a programmable value
---
--- Returns a Nat
value : Nat := 0;

--- label takes a string and applies anomaEncode to it
---
--- @return object of type Nat
label : Nat := anomaEncode "Hello world!";

--- mkHelloWorldResource constructs a Resource Object
---
--- @param nonce to avoid duplicate resources
--- @param ephemeral to indicate the resource's ephemerality, by default `false`
---
--- @return object of type Resource
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
{% endstepper %}

Next, we're going to build the Transaction function which will be used to initialize the Resource Object via transaction we manually prepare with our code.
