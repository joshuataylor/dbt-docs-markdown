# Install dbt OSS

Local development (Applies to dbt v2.0 and later)

The open-source v2 foundation is licensed under Apache 2.0. Most users don't need this page — [install dbt normally](./install-dbt.md) with the standard instructions. This page is for organizations that require the Apache 2.0 codebase specifically.

Use an isolated environment

Use `uv tool` to install `dbt-oss` in its own isolated environment. Avoid installing `dbt-oss` in the same environment as `dbt` or `dbt-core` because they all use the same entry point. If you already have `dbt` installed, refer to [Run dbt-oss alongside other dbt installs](#run-dbt-oss-alongside-other-dbt-installs).

## Install

Before installing dbt OSS, make sure you have [uv](https://docs.astral.sh/uv/getting-started/installation/) installed on your machine.

Install `dbt-oss` with `uv tool`:

```shell
uv tool install dbt-oss
```

Confirm the installed version begins with `2.`:

```shell
dbt --version
```

For adapter install details, refer to the [`dbt` repository](https://github.com/dbt-labs/dbt).

## Run `dbt-oss` alongside other dbt installs

If you have `dbt` installed and want to run `dbt-oss` alongside it, use `uvx` with a shell alias to avoid conflicts. Using `uvx` ensures dbt OSS runs in its own isolated environment without interfering with your existing dbt installation.

Add the following to your shell profile. This example uses `dbto`, but you can choose a different name:

```shell
# ~/.zshrc or ~/.bashrc
alias dbto='uvx --from dbt-oss dbt'
```

Reload your shell profile:

```shell
source ~/.zshrc   # or source ~/.bashrc
```

You can then use `dbto <command>` (or your chosen alias) instead of `dbt <command>` for dbt OSS invocations.

## What's included

* The open-source, Rust-based dbt runtime.
* The dbt project language and DAG semantics.
* The standard dbt command set (`run`, `build`, `test`, `compile`, `parse`, and more).

## What's not included

The [standard dbt install](./install-dbt.md) gives you dbt v2, which adds the following on top of the open source layer:

* SQL comprehension and static analysis
* LSP features (autocomplete, hover info, inline errors)
* `dbt lint` and error diagnostics
* dbt VS Code extension integration

For the full picture of what you get with dbt, refer to [v2 availability](../dbt/dbt-availability.md).

## Contributing

To contribute, refer to the [`dbt` repository](https://github.com/dbt-labs/dbt) and its [CONTRIBUTING guide](https://github.com/dbt-labs/dbt/blob/HEAD/CONTRIBUTING.md), or ask in the [dbt Community](../../community/resources/getting-help.md).

## License

dbt OSS is licensed under Apache 2.0. Refer to the [LICENSE file](https://github.com/dbt-labs/dbt/blob/HEAD/LICENSE) in the repository. Refer to [dbt licensing](../dbt-licensing.md?version=2.0) for more info.

## Related

* [Install dbt](./install-dbt.md) (standard install)
* [Upgrade to v2](../dbt-versions/dbt-upgrade/upgrading-to-v2.md)
* [`dbt` repository on GitHub](https://github.com/dbt-labs/dbt)
