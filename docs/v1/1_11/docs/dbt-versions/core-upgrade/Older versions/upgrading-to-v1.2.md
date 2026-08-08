# Upgrading to v1.2

Available in v1ⓘ

### Resources[​](#resources "Direct link to Resources")

* [Changelog](https://github.com/dbt-labs/dbt-core/blob/1.2.latest/CHANGELOG.md)
* [dbt Core CLI Installation guide](../../../local/install-dbt.md)
* [Cloud upgrade guide](../../upgrade-dbt-platform-version.md)

## What to know before upgrading[​](#what-to-know-before-upgrading "Direct link to What to know before upgrading")

There are no breaking changes for code in dbt projects and packages. We are committed to providing backwards compatibility for all versions 1.x. If you encounter an error upon upgrading, please let us know by [opening an issue](https://github.com/dbt-labs/dbt-core/issues/new).

### For consumers of dbt artifacts (metadata)[​](#for-consumers-of-dbt-artifacts-metadata "Direct link to For consumers of dbt artifacts (metadata)")

The manifest schema version has been updated to `v6`. The relevant changes are:

* Change to `config` default, which includes a new `grants` property with default value `{}`
* Addition of a `metrics` property, to any node which could reference metrics using the `metric()` function

For users of [state-based selection](../../../../reference/node-selection/syntax.md#about-node-selection): This release also includes new logic declaring forwards compatibility for older manifest versions. While running dbt Core v1.2, it should be possible to use `state:modified --state ...` selection against a manifest produced by dbt Core v1.0 or v1.1.

## For maintainers of adapter plugins[​](#for-maintainers-of-adapter-plugins "Direct link to For maintainers of adapter plugins")

See GitHub discussion [dbt-labs/dbt-core#5468](https://github.com/dbt-labs/dbt-core/discussions/5468) for detailed information

## New and changed functionality[​](#new-and-changed-functionality "Direct link to New and changed functionality")

* **[Grants](../../../../reference/resource-configs/grants.md)** are natively supported in `dbt-core` for the first time. That support extends to all standard materializations, and the most popular adapters. If you already use hooks to apply simple grants, we encourage you to use built-in `grants` to configure your models, seeds, and snapshots instead. This will enable you to [DRY](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) up your duplicated or boilerplate code.
* **[Metrics](../../../build/build-metrics-intro.md)** now support an `expression` type (metrics-on-metrics), as well as a `metric()` function to use when referencing metrics from within models, macros, or `expression`-type metrics. For more information on how to use expression metrics, check out the [**`dbt_metrics` package**](https://github.com/dbt-labs/dbt_metrics)
* **[dbt-Jinja functions](../../../../reference/dbt-jinja-functions-context-variables.md)** now include the [`itertools` Python module](../../../../reference/dbt-jinja-functions/modules.md#itertools), as well as the [set](../../../../reference/dbt-jinja-functions/set.md) and [zip](../../../../reference/dbt-jinja-functions/zip.md) functions.
* **[Node selection](../../../../reference/node-selection/syntax.md)** includes a [file selection method](../../../../reference/node-selection/methods.md#file) (`-s model.sql`), and [yaml selector](../../../../reference/node-selection/yaml-selectors.md) inheritance.
* **[Global configs](../../../../reference/global-configs/about-global-configs.md)** now include CLI flag and environment variable settings for [`target-path`](../../../../reference/global-configs/json-artifacts.md) and [`log-path`](../../../../reference/global-configs/logs.md), which can be used to override the values set in `dbt_project.yml`

### Specific adapters[​](#specific-adapters "Direct link to Specific adapters")

* [Postgres](../../../local/connect-data-platform/postgres-setup.md) and [Redshift](../../../local/connect-data-platform/redshift-setup.md) profiles support a `retries` config, if dbt encounters an operational error or timeout when opening a connection. The default is 1 retry.
