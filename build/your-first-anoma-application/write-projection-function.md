---
hidden: true
---

# Write Projection Function

Let's enrich our HelloWorld application by adding functionality that allows us to read the label we have just created.

To achieve this, we add `getLabel`, which is a special type of function, a projection function. You can think of it as a read function and it lives within the interface \[<mark style="background-color:orange;">LINK TO MODEL VIEW CONTROLLER VISUAL</mark>]. Thus, we create a separate folder, "Interface", and add our new projection function file in there.

<mark style="background-color:orange;">TODO: Remove Interface folder in docs, code has been adjusted to flat structure w/o folder</mark>

{% code title="~/HelloWorld" %}
```bash
mkdir Interface
cd Interface/
touch Projection.juvix
```
{% endcode %}

We can now specify `getLabel`. It will take a parameter of type `Resource` and then access its label via `label`. We finally retrieve the underlying `String` type by applying `anomaDecode` on the `label` of our Resource.

{% code title="~/Projection.juvix" %}
```agda
module Projection;

import Stdlib.Prelude open;
import Anoma open;

--- getLabel retrieves the label from a given Resource
---
--- @param resource to extract the label from
---
--- @return object of type String
getLabel (resource : Resource) : String :=
   anomaDecode (Resource.label resource);
```
{% endcode %}
