---
description: This page explains how to run your application locally.
---

# Run your App

Amazing, we have written the code we need to compile a `HelloWorld` resource object and can create a transaction to initialize our `HelloWorld` resource. Let's run through these steps by starting with the setup of the local Anoma Client and Node.

### Anoma Client and Node

{% hint style="warning" %}
The following way of running the Anoma Client and Node are temporary and thus, Work-In-Progress areas which are specific to the **current stage of devnet**.
{% endhint %}

First and foremost, we need a local version of the `artem/juvix-node-integration-v0.28` branch of [Anoma's codebase](https://github.com/anoma/anoma/tree/testnet-01), install its dependencies, and compile the code. For a detailed description on how to do these steps, please follow this [README](https://github.com/anoma/anoma/blob/testnet-01/README.md).

{% hint style="danger" %}
The recommended branch will soon change to `testnet-01` , once integrations have been merged.
{% endhint %}

Assuming you have successfully compiled the Anoma codebase, you can use the following `start-config.exs`and `makefile` to start the Anoma Client and Anoma Node.

First, copy the following code into a file you call `start-config.exs` at your project root:

```elixir
eclient = Anoma.Client.Examples.EClient.create_example_client
IO.puts("#{eclient.client.grpc_port} #{eclient.node.node_id}")
Logger.configure(level: :debug)
Anoma.Node.Utility.Consensus.start_link(node_id: eclient.node.node_id, interval: 5000)
File.write("config.yaml", "url: localhost\nport: #{eclient.client.grpc_port}\nnodeid: \"\"")
```

Now, copy the following code into a `makefile`, again at your project root.

<pre class="language-makefile"><code class="lang-makefile"># how to run:
# 0. set anoma-path variable in this file to the folder where anoma is cloned and compiled (branch origin/artem/juvix-node-integration-v0.28)
# 1. copy this makefile code in your own makefile at the root of your project
# 2. make run-client
# 3. make run-node # keep this running in a separate terminal
# 4. make add-transaction
anoma-path = <a data-footnote-ref href="#user-content-fn-1">~/Code/Anoma/devnet/anoma</a>
base = Transaction


juvix = $(base).juvix
nockma = $(base).nockma
proved = $(base).proved.nockma
root = $(shell pwd)
config = config.yaml
anoma-config = $(anoma-path)/$(config)

all: $(proved)

.PHONY: clean
clean:
	rm -rf $(config) $(proved) $(nockma)

.PHONY: run-client
run-client:
	juvix dev anoma start --anoma-dir $(anoma-path)

.PHONY: run-node
run-node:
	cd $(anoma-path) &#x26;&#x26; \
		mix run --no-halt $(root)/start-config.exs

.PHONY: add-transaction
add-transaction: $(proved) $(config)
	juvix dev anoma -c $(config) add-transaction $(proved)

$(config): $(anoma-config)
	cp $(anoma-config) $(config)

$(nockma): $(juvix)
	juvix compile anoma $(juvix)

$(proved): $(nockma) $(config)
	juvix dev anoma -c $(config) prove $(nockma)

</code></pre>

Open a new terminal in your project root:

{% code title="HelloWorld" %}
```bash
make run-client
make run-node # keep this running in a separate terminal
```
{% endcode %}

### Calling the Transaction Function

Now, once the Anoma Client and Node have started in your first terminal which you will keep open, we want to call our transaction function. We do so by opening a new terminal and running the following `makefile` command:

{% code title="HelloWorld" %}
```bash
make add-transaction
```
{% endcode %}

The above command will trigger our code to compile and get proved.

***

:tada: **Congrats! You have successfully built and deployed your first Resource Object. We will soon add more functionality, e.g. to index your Resource and make use of custom Projection functions.**

[^1]: Change this to your own anoma-path
