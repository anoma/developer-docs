# State

Resources as atomic units of state and can be used to model all kinds of objects such as fruits <kbd>🍏</kbd>, currencies <kbd>💵</kbd>, messages <kbd>💌</kbd> or anything else you might think of. The underlying resource object contains standardized data fields, which are defined in the [Anoma specs](https://specs.anoma.net/latest/arch/system/state/resource_machine/data_structures/resource/index.html). While all fields are needed for the [Anoma Resource Machine](../page/) the **label**, **quantity**, and **value** field are particularly relevant for application developers.

<table data-header-hidden><thead><tr><th width="223"></th><th align="center"></th><th align="center"></th></tr></thead><tbody><tr><td><kbd>10 💵  {owner: Alice}</kbd></td><td align="center"><kbd>2 🍏 {fruitiness: 8/10}</kbd></td><td align="center"><kbd>1 💌 {text: "I ❤️ u"}</kbd></td></tr><tr><td><code>10 USD</code> resource</td><td align="center"><code>2 Apple</code> resource</td><td align="center"><code>1 Message</code> Resource</td></tr></tbody></table>

## Label

The label can contain arbitrary data describing the resource and determining its [kind](kind.md). Examples for label data are the name and symbol of a currency, the species of an apple, or a message associated with a specific channel in a messaging application.

## Value

The value field can contain arbitrary data associated with this specific resource.  Examples for value data are information about the owner, the fruitiness of a fruit, or the text in a message. The difference to the label field is that this data is not influencing the[ resource kind](kind.md).

## Quantity

The quantity indicates the number of units that the resource represents. In practice, resources are often split and joined. For example, Alice owning a 10 USD resource might want to give 4 USD to pay someone and keep the remaining 6 for herself. In some cases, only one resource instance should ever exist (e.g., for a message or an NFT).
