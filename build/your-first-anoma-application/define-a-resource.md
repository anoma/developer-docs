---
description: >-
  In this page, we're defining a resource object and giving it a custom label,
  "Hello World!".
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

-- We import the standard library prelude as well as the Applib library.
import Stdlib.Prelude open;
import Applib open;
```
{% endcode %}

We now start defining our resource which we call `mkHelloWorldResource` by giving it values for `logic`, `label`, `value`, `quantity`, `ephemeral`, `nonce`, `randSeed` and `nullifierKeyCommitment`. You can find more detailed information on the resource and its parameters in [resources](../../learn/resources/ "mention").

Let's start by putting default values for `logic` and `value` and pass the `message` parameter to `label`.

{% code title="HelloWorld.juvix" overflow="wrap" %}
```agda
-- ### Module, imports ###

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
  };
```
{% endcode %}

In the context of our HelloWorld application, this implies that we're passing the "Hello World!"  `message` when we're creating the resource. This allows us to set any string, like "Hello Anomages!". In addition to that we assign `true` to the `logic`, implying that this resource will not have any logic constraints. Similarly, we assign zero for `value` which is an arbitrary default.

Let's continue adding missing parameters:

{% code title="HelloWorld.juvix" overflow="wrap" %}
```agda
-- ### Module, imports, and logic ###

mkHelloWorldResource
  (nonce : Nat) (message : String) {ephemeral : Bool := false} : Resource :=
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
```
{% endcode %}

</details>

Next, we're going to build the transaction function which will be used to initialize the resource object via transaction we manually prepare with our code.
