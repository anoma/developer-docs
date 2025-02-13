# Write a Projection Function

Let's enrich our HelloWorld application by adding functionality that allows us to read the label we have just created.

To achieve this, we add a special type of function, a projection function. You can think of it as a read function and you can find more details about it under [interface.md](../../learn/applications/interface.md "mention").

We're adding a new file to accomodate our projection function.

{% code title="~/HelloWorld/" %}
```bash
touch GetMessage.juvix
```
{% endcode %}

We can now specify `GetMessage.juvix`. It will take an `encodedResource` parameter of type `Nat`. As the name suggests, this is an encoded resource object which we will first apply `anomaDecode` to, then access its label via `Resource.label`. We finally retrieve the underlying `String` type by applying another `anomaDecode`.

{% code title="GetMessage.juvix" %}
```agda
module GetMessage;

import Stdlib.Prelude open;
import Applib open;

--- Extract the message from a HelloWorld ;Resource;
main (encodedResource : Nat) : String :=
  encodedResource |> builtinAnomaDecode |> Resource.label |> builtinAnomaDecode;
```
{% endcode %}

In the following chapter, we will compile and "deploy" our code locally.
