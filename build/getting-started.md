---
description: Install Juvix in less than 5 minutes.
icon: screwdriver-wrench
---

# Getting Started

Juvix is Anoma's language for intent-centric and declarative decentralized applications.&#x20;

Get started by installing Juvix in less than 5 minutes.

## Installing Juvix

Please select from the options below to install the Juvix compiler on your system. We currently support

* Linux `x86_64`
* macOS `x86_64` and `aarch64 (M1/M2)`

{% hint style="info" %}
All example applications are currently compliant with the `anoma-apps` [v0.4.1 release](https://github.com/anoma/anoma-apps/releases/tag/v0.4.1).
{% endhint %}

{% stepper %}
{% step %}
### Prerequisites

The Juvix compiler requires [the LLVM/clang compiler](https://llvm.org) to be available on your system `PATH` in order to build native binaries.

{% tabs %}
{% tab title="macOS" %}
If you use the [Homebrew package manager](https://brew.sh), you can install LLVM/clang via

<pre class="language-bash"><code class="lang-bash"><strong>brew update
</strong>brew install llvm
</code></pre>

Alternatively, you can install the [Xcode command line tools](https://developer.apple.com/xcode/resources/)

```bash
xcode-select install
```
{% endtab %}

{% tab title="Linux" %}
On **Ubuntu/Debian**, install clang via

```bash
sudo apt-get update
sudo apt install clang
```

On **Arch Linux**, install clang via

```bash
sudo pacman -Syu
sudo pacman -S clang
```
{% endtab %}
{% endtabs %}
{% endstep %}

{% step %}
### Juvix Compiler

Next, you need to install the Juvix compiler.

{% tabs %}
{% tab title="macOS" %}
If you use the [Homebrew package manager,](https://brew.sh) you can use our tap:

```sh
brew update
brew tap anoma/juvix
brew install --overwrite juvix-nightly
```

Alternatively, you can download and unzip the compiler executable linked below

* [macOS x86\_64](https://github.com/anoma/juvix-nightly-builds/releases/download/nightly-2025-05-06-0.6.10-682a6fa/juvix-darwin-x86_64.tar.gz)
* [macOS aarch64 (M1/M2)](https://github.com/anoma/juvix-nightly-builds/releases/download/nightly-2025-05-06-0.6.10-682a6fa/juvix-darwin-aarch64.tar.gz)

and move it to a directory on your shell's `PATH (e.g., /usr/local/bin`).
{% endtab %}

{% tab title="Linux" %}
Download and unzip the compiler executable linked below

* [Linux x86\_64](https://github.com/anoma/juvix-nightly-builds/releases/download/nightly-2025-05-06-0.6.10-682a6fa/juvix-linux-x86_64.tar.gz)

and move it to a directory on your shell's `PATH (e.g., /usr/local/bin`).
{% endtab %}
{% endtabs %}
{% endstep %}

{% step %}
### IDE Setup

Lastly, you can set up the IDE of your choice.

{% tabs %}
{% tab title="Visual Studio Code" %}
First install visual studio code from the [Microsoft website](https://code.visualstudio.com/download).

In the extension tab search for `juvix` and install the extension that features the Tara logo.

See the [vscode-juvix repository](https://github.com/anoma/vscode-juvix) for usage information.
{% endtab %}

{% tab title="Emacs" %}
Clone the [juvix-mode repository](https://github.com/anoma/juvix-mode.git)

```shell
git clone https://github.com/anoma/juvix-mode.git
```

And add the following lines to your Emacs configuration file:

```emacs-lisp
(push "/path/to/juvix-mode/" load-path)
(require 'juvix-mode)
```

See the [juvix-mode repository](https://github.com/anoma/juvix-mode.git) for usage information.
{% endtab %}
{% endtabs %}
{% endstep %}
{% endstepper %}

{% hint style="info" %}
We will continuously add support for more platforms and IDEs .
{% endhint %}

## Juvix Documentation

The Juvix documentation is available at [https://docs.juvix.org](https://docs.juvix.org).
