---
description: This page explains how to run your application locally.
hidden: true
---

# Run your App

Amazing, we have written the code we need to compile a `HelloWorld` resource object and can create a transaction to initialize our `HelloWorld` resource. Let's run through these steps by starting with the setup of the local Anoma Client and Node.

### Anoma Client and Node

{% hint style="warning" %}
The following way of running the Anoma Client and Node are temporary and thus, Work-In-Progress areas which are specific to the **current stage of the devnet**.
{% endhint %}

First and foremost, we need a local version of the `6a07f590a5a1addb52fa7f6e39dd9315accc93c5` commit of [Anoma's codebase](https://github.com/anoma/anoma/commit/6a07f590a5a1addb52fa7f6e39dd9315accc93c5), install its dependencies, and compile the code. For a detailed description on how to do these steps, please follow this [README](https://github.com/anoma/anoma/blob/base/README.md).

{% hint style="danger" %}
As we continue to work towards a stable testnet release, we will further update recommended commits of the Anoma codebase.
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
root = $(shell pwd)

ifneq ($(XDG_CONFIG_HOME),)
    juvix-cmd = XDG_CONFIG_HOME=$(XDG_CONFIG_HOME) juvix
else
    juvix-cmd = juvix
endif

anoma-build-dir = anoma-build
anoma-build = $(anoma-build-dir)/.exists

config = $(anoma-build-dir)/config.yaml

temp-file := $(anoma-build-dir)/temp_line

juvix = $(base-path)/$(base).juvix
nockma = $(anoma-build-dir)/$(base).nockma
proved = $(anoma-build-dir)/$(base).proved.nockma

get-message-juvix = $(base-path)/$(get-message).juvix
get-message-nockma = $(anoma-build-dir)/$(get-message).nockma
get-message-proved = $(anoma-build-dir)/$(get-message).proved.nockma

unspent-resources = $(anoma-build-dir)/unspent-resources
last-message-txt = $(anoma-build-dir)/last-message.txt

port = $(anoma-build-dir)/port
host = $(anoma-build-dir)/host

$(anoma-build):
	mkdir -p $(anoma-build-dir)
	touch $(anoma-build)

.PHONY: clean
clean:
	juvix clean
	rm -rf $(anoma-build-dir)

.PHONY: anoma-start
anoma-start:
	rm -f $(config)
ifdef ANOMA_DEBUG
	cd $(ANOMA_PATH) && \
		mix phx.server --no-halt $(root)/../start-config.exs
else
	$(juvix-cmd) dev anoma start --force --anoma-dir $(ANOMA_PATH)
endif

.PHONY: anoma-stop
anoma-stop:
ifdef ANOMA_DEBUG
	@echo "ANOMA_DEBUG is incompatible with anoma-stop" && false
else
	$(juvix-cmd) dev anoma stop
endif

.PHONY: $(message-file)
$(message-file): $(anoma-build)
	@echo $(message) \
	> $(message-file)

$(config): $(anoma-build)
ifdef ANOMA_DEBUG
	cp $(ANOMA_PATH)/config.yaml $(config)
else
	$(juvix-cmd) dev anoma print-config > $(config)
endif

.PHONY: add-transaction
add-transaction: $(proved) $(config)
	$(juvix-cmd) dev anoma -c $(config) add-transaction $(proved)

.PHONY: get-last-message
get-last-message: $(last-message-txt)
	@cat $(last-message-txt)

.PHONY: $(last-message-txt)
$(last-message-txt): $(get-message-nockma) $(config) $(unspent-resources)
	tail -n 1 $(unspent-resources) > $(temp-file)
	$(juvix-cmd) dev anoma -c $(config) prove $(get-message-nockma) -o $(get-message-proved) --arg 'base64:$(temp-file)'
	$(juvix-cmd) dev nockma encode --cue --from bytes --to bytes < anoma-build/GetMessage.proved.nockma > $(last-message-txt)

$(nockma): $(juvix) $(anoma-build)
	$(juvix-cmd) compile anoma $(juvix) -o $(nockma)

$(proved): $(nockma) $(config)
	$(juvix-cmd) dev anoma -c $(config) prove $(nockma) -o $(proved)

$(get-message-nockma): $(anoma-build) $(get-message-juvix)
	$(juvix-cmd) compile anoma $(get-message-juvix) -o $(get-message-nockma)

.PHONY: $(unspent-resources)
$(unspent-resources): $(anoma-build) $(host) $(port)
	curl -X GET $$(cat $(host)):$$(cat $(port))/indexer/unspent-resources | jq -r '.unspent_resources[-1] // error("no messages exist")' > $(unspent-resources)

$(host): $(config)
	yq -r '.url' $(config) | tr -d '\n' > $(host)

$(port): $(config)
	yq -r '.port' $(config) | tr -d '\n' > $(port)
```

Importantly, we first need to set the environment variable `$ANOMA_PATH` to the path of the Anoma node clone (use something like `export ANOMA_PATH=...` in the terminal to set it).

```bash
# Set path to the folder where Anoma is cloned and compiled
# Example:
# export ANOMA_PATH=~/anoma/anoma
export ANOMA_PATH=#INSERT_PATH_TO_ANOMA_FOLDER
```

Open a new terminal in your project root:

{% code title="HelloWorld" %}
```bash
make anoma-start # start the Anoma node
```
{% endcode %}

### Calling the Transaction Function

Now, once the Anoma Client and Node have started in your first terminal which you will keep open, we want to call our transaction function. We do so by opening a new terminal and running the following `makefile` command:

{% code title="HelloWorld" %}
```bash
make add-transaction # Submit the HelloWorld transaction to anoma
make get-last-message # Get the message from the latest HelloWorld Resource
# ...
make anoma-stop # stop the Anoma node when you've finished
make clean # clean up after yourself
```
{% endcode %}

***

:tada: **Congrats! You have successfully built and deployed your first Resource Object. We will soon add more functionality, e.g. to index your Resource and make use of custom Projection functions.**
