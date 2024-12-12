---
description: This chapter explains how to run your HelloWorld application locally.
---

# Run your App

Amazing, we have written the code we need to compile a HelloWorld Resource Object, create a transaction to initialize our HelloWorld resource, and finally we can read its label via our projection function. Let's run through these steps by starting with the setup of the local Anoma Client and Node.

### Anoma Client and Node

{% hint style="warning" %}
The following way of running the Anoma Client and Node are temporary and thus, Work-In-Progress areas which are specific to the **current stage of devnet**.
{% endhint %}

First and foremost, we need a local version of the `artem/juvix-node-integration-v0.28` branch of [Anoma's codebase](https://github.com/anoma/anoma/tree/testnet-01), install its dependencies, and compile the code. For a detailed description on how to do these steps, please follow this [README](https://github.com/anoma/anoma/blob/testnet-01/README.md).

{% hint style="danger" %}
The recommended branch will soon change to `testnet-01` , once integrations have been merged.
{% endhint %}

Assuming you have successfully compiled the Anoma codebase, you can use the following `makefile` to start the Anoma Client and Anoma Node.

{% file src="../../.gitbook/assets/makefile" %}
Makefile to start Anoma Client and Anoma Node, copy in project root
{% endfile %}

Open a new terminal in your project root:

{% code title="~/HelloWorld" %}
```bash
make run-client-and-node
```
{% endcode %}

### Calling the Transaction Function

Now, once the Anoma Client and Node have started in your first terminal which you will keep open, we want to call our transaction function. We do so by opening a new terminal and running the following `makefile` command:

{% code title="~/HelloWorld" %}
```bash
make add-transaction
```
{% endcode %}

The above command will trigger our code to compile and get proved.

***

:tada: **Congrats! You have successfully built and deployed your first Resource Object. We will soon add more functionality, e.g. to index your Resource and make use of custom Projection functions.**
