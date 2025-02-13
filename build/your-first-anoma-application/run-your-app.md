---
description: This page explains how to run your application locally.
---

# Run your App

Amazing, we have written the code we need to compile a `HelloWorld` resource object and can create a transaction to initialize our `HelloWorld` resource. Let's run through these steps by starting with the setup of the local Anoma Client and Node.

### Anoma Client and Node

{% hint style="warning" %}
The following way of running the Anoma Client and Node are temporary and thus, Work-In-Progress areas which are specific to the **current stage of the devnet**.
{% endhint %}

First and foremost, we need a local version of the `artem/juvix-node-integration-v0.28` branch of [Anoma's codebase](https://github.com/anoma/anoma/tree/artem/juvix-node-integration-v0.28), install its dependencies, and compile the code. For a detailed description on how to do these steps, please follow this [README](https://github.com/anoma/anoma/blob/artem/juvix-node-integration-v0.28/README.md).

{% hint style="danger" %}
The recommended branch will soon change to `testnet-01` , once integrations have been merged.
{% endhint %}

Before we can create the necessary `makefile`, we need to install its dependencies.

{% tabs %}
{% tab title="macOS" %}
If you use the [Homebrew package manager](https://brew.sh), you can install al dependencies via

```bash
brew install jq
brew install yq
brew install grpcurl
```
{% endtab %}

{% tab title="Linux" %}
On **Ubuntu/Debian**, install clang via

<pre class="language-bash"><code class="lang-bash"># jq
sudo apt-get install jq
# yq
sudo add-apt-repository ppa:rmescandon/yq
sudo apt update
sudo apt install yq -y
<strong># grpcurl
</strong># Please build from source via https://github.com/fullstorydev/grpcurl/releases
</code></pre>

On **Arch Linux**, install clang via

```bash
sudo pacman -S jq
sudo pacman -S yq
# Please build grpcurl from source via https://github.com/fullstorydev/grpcurl/releases
```
{% endtab %}
{% endtabs %}

Assuming you have successfully compiled the Anoma codebase and installed the above dependencies, you can now use the following `makefile` to start the Anoma Client and Anoma Node. Copy the following code into a `makefile` at your project root.

{% hint style="warning" %}
Please mind that the `makefile` is temporary and an **artifact of the current stage of the devnet**.
{% endhint %}

```makefile
ANOMA_PATH ?= $(error set the ANOMA_PATH variable to a path to an anoma clone)
base-path = .
base = HelloWorld
get-message = GetMessage

anoma-build-dir = anoma-build
anoma-build = $(anoma-build-dir)/.exists

config = $(anoma-build-dir)/config.yaml

juvix = $(base-path)/$(base).juvix
nockma = $(anoma-build-dir)/$(base).nockma
proved = $(anoma-build-dir)/$(base).proved.nockma

get-message-juvix = $(base-path)/$(get-message).juvix
get-message-nockma = $(anoma-build-dir)/$(get-message).nockma
get-message-proved = $(anoma-build-dir)/$(get-message).proved.nockma

last-unspent-resource = $(anoma-build-dir)/last-unspent-resources

port = $(anoma-build-dir)/port
host = $(anoma-build-dir)/host

$(anoma-build):
	@mkdir -p $(anoma-build-dir)
	@touch $(anoma-build)

.PHONY: clean
clean:
	@juvix clean
	@rm -rf $(anoma-build-dir)

.PHONY: start-anoma
start-anoma:
	@juvix dev anoma start --anoma-dir $(ANOMA_PATH)

.PHONY: stop-anoma
stop-anoma:
	@juvix dev anoma stop

$(config): $(anoma-build)
	@juvix dev anoma print-config > $(config)

.PHONY: add-transaction
add-transaction: $(proved) $(config)
	@juvix dev anoma -c $(config) add-transaction $(proved)

.PHONY: get-message
get-message: $(get-message-nockma) $(config) $(last-unspent-resource)
	@juvix dev anoma -c $(config) prove $(get-message-nockma) -o $(get-message-proved) --arg 'base64:$(last-unspent-resource)'
	@juvix dev nockma encode --cue --from bytes --to bytes < anoma-build/GetMessage.proved.nockma

$(nockma): $(juvix) $(anoma-build)
	@juvix compile anoma $(juvix) -o $(nockma)

$(proved): $(nockma) $(config)
	@juvix dev anoma -c $(config) prove $(nockma) -o $(proved)

$(get-message-nockma): $(anoma-build) $(get-message-juvix)
	@juvix compile anoma $(get-message-juvix) -o $(get-message-nockma)

.PHONY: $(last-unspent-resource)
$(last-unspent-resource): $(anoma-build) $(host) $(port)
	@grpcurl -plaintext $$(cat $(host)):$$(cat $(port)) Anoma.Protobuf.IndexerService.ListUnspentResources | jq -r '.unspentResources[-1] // error("no messages exist")' > $(last-unspent-resource)

$(host): $(config)
	@yq -r '.url' $(config) | tr -d '\n' > $(host)

$(port): $(config)
	@yq -r '.port' $(config) | tr -d '\n' > $(port)
```

Importantly, we first need to set the environment variable `$ANOMA_PATH` to the path of the Anoma node clone (use something like `export ANOMA_PATH=...` in the terminal to set it).

```bash
# Set path to the folder where Anoma is cloned and compiled
# Example:
# export ANOMA_PATH=~/anoma/HelloWorld
export ANOMA_PATH=#INSERT_PATH_TO_ANOMA_FOLDER
```

Open a new terminal in your project root:

{% code title="HelloWorld" %}
```bash
make start-anoma # start the Anoma node
```
{% endcode %}

### Calling the Transaction Function

Now, once the Anoma Client and Node have started in your first terminal which you will keep open, we want to call our transaction function. We do so by opening a new terminal and running the following `makefile` command:

{% code title="HelloWorld" %}
```bash
make add-transaction # Submit the HelloWorld transaction to anoma
make get-message # Get the message from the latest HelloWorld Resource
# ...
make stop-anoma # stop the Anoma node when you've finished
make clean # clean up after yourself
```
{% endcode %}

***

:tada: **Congrats! You have successfully built and deployed your first Resource Object. We will soon add more functionality, e.g. to index your Resource and make use of custom Projection functions.**
