---
description: >-
  A tutorial setting you up to build, run, and test your first Anoma
  application.
icon: rocket-launch
---

# Your First Anoma App

## Prerequisites

Before creating your first Anoma dApp, make sure to have followed the [getting-started.md](../getting-started.md "mention") instructions.

## Building `HelloWorld`&#x20;

The goal of this tutorial is to&#x20;

1. Define a "Hello World!" [Resource](../../learn/resources/).
2. Generate proofs
3. Generate the transaction object
4. Submit a Protocol Adapter transaction

### Create a New Project

We start by cloning the `anoma-beta-documentation` repo and then want to go to the `examples/` folder to find the `hello-world` example.

```bash
git clone https://github.com/anoma/anoma-beta-documentation.git
cd examples/hello-world
```

Once we have executed above code, let's run the `hello-world` scaffolding code before we start expanding on it.

```bash
cargo run
```

Great! We can now start with the actual `hello-world` code. Head over to [define-a-resource.md](define-a-resource.md "mention").
