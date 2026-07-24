# Install dbt Core 2.0

Local developmentⓘ

dbt Core 2.0 is in alpha

dbt Core 2.0 is under active development and not recommended for production use. Features and APIs may change before the stable release.

dbt Core 2.0 is the open-source foundation behind Fusion, licensed under Apache 2.0. Most users don't need this page — [install dbt normally](https://docs.getdbt.com/docs/local/install-dbt.md) with the standard instructions. This page is for organizations that require the Apache 2.0 codebase specifically.

## Install[​](#install "Direct link to Install")

Install the dbt Core 2.0 prerelease with `pip`:

```shell
python -m pip install --pre dbt-core
```

Confirm the installed version begins with `2.`:

```shell
dbt --version
```

During alpha, you must target either the pre-release version or an explicit pin. After install, immediately update to the most recent version:

Explicit pin:

`python -m pip install dbt-core==2.0.0-alpha.1`

For adapter install details, refer to the [`dbt-core` repository](https://github.com/dbt-labs/dbt-core).

## What's included[​](#whats-included "Direct link to What's included")

* The open-source, Rust-based dbt runtime.
* The dbt project language and DAG semantics.
* The standard dbt command set (`run`, `build`, `test`, `compile`, `parse`, and more).

## What's not included[​](#whats-not-included "Direct link to What's not included")

The [standard dbt install](https://docs.getdbt.com/docs/local/install-dbt.md) gives you Fusion, which adds the following to dbt Core 2.0:

* SQL comprehension and static analysis
* LSP features (autocomplete, hover info, inline errors)
* `dbt lint` and error diagnostics
* dbt VS Code extension integration

For the full picture of what you get with dbt, refer to [Fusion availability](https://docs.getdbt.com/docs/fusion/fusion-availability.md).

## Contributing[​](#contributing "Direct link to Contributing")

dbt Core 2.0 is developed in the open. To contribute, refer to the [`dbt-core` repository](https://github.com/dbt-labs/dbt-core) and its [CONTRIBUTING guide](https://github.com/dbt-labs/dbt-core/blob/HEAD/CONTRIBUTING.md), or ask in the [dbt Community](https://docs.getdbt.com/community/resources/getting-help.md).

## License[​](#license "Direct link to License")

dbt Core 2.0 is licensed under Apache 2.0. Refer to the [LICENSE file](https://github.com/dbt-labs/dbt-core/blob/HEAD/License.md) in the repository. Refer to [dbt licensing](https://docs.getdbt.com/docs/dbt-licensing.md?version=2.0) for more info.

## Related[​](#related "Direct link to Related")

* [Install dbt](https://docs.getdbt.com/docs/local/install-dbt.md) (standard install)
* [Upgrade to v2](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md)
* [`dbt-core` repository on GitHub](https://github.com/dbt-labs/dbt-core)
