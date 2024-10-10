---
hidden: true
---

# Installation Steps

First, macOS homebrew

```
brew tap anoma/juvix
```

```
brew install juvix
```

Now, getting info with

```
juvix doctor
```

give me following output:

```
> Checking for clang...
> Checking clang version...
> Checking for wasm-ld...
  ! Could not find the wasm-ld command
  ! https://docs.juvix.org/0.6.6/reference/tooling/doctor/#could-not-find-the-wasm-ld-command
> Checking that clang supports wasm32...
  ! Clang does not support the wasm32 target
  ! https://docs.juvix.org/0.6.6/reference/tooling/doctor/#clang-does-not-support-the-wasm32-target
> Checking that clang supports wasm32-wasi...
  ! Clang does not support the wasm32-wasi target
  ! https://docs.juvix.org/0.6.6/reference/tooling/doctor/#clang-does-not-support-the-wasm32-wasi-target
> Checking that WASI_SYSROOT_PATH is set...
  ! Environment variable WASI_SYSROOT_PATH is missing
  ! https://docs.juvix.org/0.6.6/reference/tooling/doctor/#environment-variable-wasi_sysroot_path-is-not-set
> Checking for wasmer...
  ! Could not find the wasmer command
  ! https://docs.juvix.org/0.6.6/reference/tooling/doctor/#could-not-find-the-wasmer-command
> Checking latest Juvix release on Github...
```

^ Can we maybe make it clear if a step was successful? This look like just outputting when something is missing

After installing dependencies for `wasi-sdk` , my `juvix doctor` output is the following:

```
> Checking for clang...
> Checking clang version...
> Checking for wasm-ld...
  ! Could not find the wasm-ld command
  ! https://docs.juvix.org/0.6.6/reference/tooling/doctor/#could-not-find-the-wasm-ld-command
> Checking that clang supports wasm32...
  ! Clang does not support the wasm32 target
  ! https://docs.juvix.org/0.6.6/reference/tooling/doctor/#clang-does-not-support-the-wasm32-target
> Checking that clang supports wasm32-wasi...
  ! Clang does not support the wasm32-wasi target
  ! https://docs.juvix.org/0.6.6/reference/tooling/doctor/#clang-does-not-support-the-wasm32-wasi-target
> Checking that WASI_SYSROOT_PATH is set...
> Checking for wasmer...
  ! Could not find the wasmer command
  ! https://docs.juvix.org/0.6.6/reference/tooling/doctor/#could-not-find-the-wasmer-command
> Checking latest Juvix release on Github...
```

Now I have installed dependencies but apparently, miss some dependencies. Seems like I'm required to separately install the prerequisites:

For `wasmer`:

```
curl https://get.wasmer.io -sSfL | sh
```

That worked after closing and opening the terminal

`juvix doctor` output:

```
> Checking for clang...
> Checking clang version...
> Checking for wasm-ld...
  ! Could not find the wasm-ld command
  ! https://docs.juvix.org/0.6.6/reference/tooling/doctor/#could-not-find-the-wasm-ld-command
> Checking that clang supports wasm32...
  ! Clang does not support the wasm32 target
  ! https://docs.juvix.org/0.6.6/reference/tooling/doctor/#clang-does-not-support-the-wasm32-target
> Checking that clang supports wasm32-wasi...
  ! Clang does not support the wasm32-wasi target
  ! https://docs.juvix.org/0.6.6/reference/tooling/doctor/#clang-does-not-support-the-wasm32-wasi-target
> Checking that WASI_SYSROOT_PATH is set...
  ! Environment variable WASI_SYSROOT_PATH is missing
  ! https://docs.juvix.org/0.6.6/reference/tooling/doctor/#environment-variable-wasi_sysroot_path-is-not-set
> Checking for wasmer...
> Checking latest Juvix release on Github...
```

***

According to Paul, for macOS, only `clang` is a prerequisite, followed by the homebrew installation steps

^ added this to the docs



Now trying to create first application with the installation steps from above.
