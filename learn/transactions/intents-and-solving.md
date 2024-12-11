# Intents & Solving

{% hint style="warning" %}
This page is WIP.
{% endhint %}

An unbalanced transaction is called an **intent** and requires a counterparty to provide a matching.

Anoma users submit their intents to the intent gossip network in the form of unbalanced transactions, which are received and processed by solvers that output balanced ARM transactions. These transactions are then ordered and finally sent to the executor node, that verifies and executes the transactions in the determined order, updating the state.

{% hint style="info" %}
For the current private devnet, the Anoma node is running a brute-force solver. The solver tries to find matching intents in the intent pool by composing them and checking if the resulting transaction is valid.
{% endhint %}

