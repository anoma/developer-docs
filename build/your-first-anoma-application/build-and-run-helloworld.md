# Build and Run HelloWorld

Amazing, we have written the code we need to compile a HelloWorld Resource Object, create a transaction to initialize our HelloWorld resource, and finally we can read its label via our projection function. Let's run through these steps by starting with the setup of the local Anoma Client and Node.

### Anoma Client and Node

{% hint style="warning" %}
The following way of running the Anoma Client and Node are temporary and thus, Work-In-Progress areas which are specific to the **current stage of devnet**.
{% endhint %}

First and foremost, we need a local version of the `testnet-01` branch of [Anoma's codebase](https://github.com/anoma/anoma/tree/testnet-01), install its dependencies, and compile the code. For a detailed description on how to do these steps, please follow this [README](https://github.com/anoma/anoma/blob/testnet-01/README.md).

<mark style="background-color:orange;">TODO: Make sure test</mark> <mark style="background-color:orange;"></mark><mark style="background-color:orange;">`testnet-01`</mark><mark style="background-color:orange;">branch is working by testing on clean device</mark>

Assuming you have successfully compiled the Anoma codebase, you can use the following `makefile` to start the Anoma Client and Anoma Node.

{% file src="../../.gitbook/assets/makefile" %}
Makefile to start Anoma Client and Anoma Node, copy in project root
{% endfile %}

<mark style="background-color:orange;">TODO: Change</mark> <mark style="background-color:orange;"></mark><mark style="background-color:orange;">`makefile`</mark><mark style="background-color:orange;">to assume flat folder structure and one command for both client and node</mark>

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

### Calling the Projection Function

The last step of what we would like to do with our HelloWorld resource is retrieve its label by calling the projection function.

<mark style="background-color:orange;">TODO: Add call to projection function</mark>
