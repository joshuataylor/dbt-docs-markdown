# Install dbt OSS

Local development (Applies to dbt v2.0 and later)

The open-source v2 foundation is licensed under Apache 2.0. Most users don't need this page — [install dbt normally](./install-dbt.md) with the standard instructions. This page is for organizations that require the Apache 2.0 codebase specifically.

## Install

Install the dbt OSS prerelease with `pip`:

```shell
python -m pip install --pre dbt-core
```

Confirm the installed version begins with `2.`:

```shell
dbt --version
```

During beta, you must target either the pre-release version or an explicit pin. After install, immediately update to the most recent version:

Explicit pin:

`python -m pip install dbt-core==2.0.0rc2`

For adapter install details, refer to the [`dbt` repository](https://github.com/dbt-labs/dbt).

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
