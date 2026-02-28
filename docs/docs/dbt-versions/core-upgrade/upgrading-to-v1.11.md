# Upgrading to v1.11

## Resources[​](#resources "Direct link to Resources")

* [dbt Core v1.11 changelog](https://github.com/dbt-labs/dbt-core/blob/1.11.latest/CHANGELOG.md)
* [dbt Core CLI Installation guide](https://docs.getdbt.com/docs/core/installation-overview.md)
* [Cloud upgrade guide](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-version-in-cloud.md#release-tracks)

## What to know before upgrading[​](#what-to-know-before-upgrading "Direct link to What to know before upgrading")

dbt Labs is committed to providing backward compatibility for all versions 1.x. Any behavior changes will be accompanied by a [behavior change flag](https://docs.getdbt.com/reference/global-configs/behavior-changes.md#behavior-change-flags) to provide a migration window for existing projects. If you encounter an error upon upgrading, please let us know by [opening an issue](https://github.com/dbt-labs/dbt-core/issues/new).

Starting in 2024, dbt provides the functionality from new versions of dbt Core via [release tracks](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md) with automatic upgrades. If you have selected the **Latest** release track in dbt, you already have access to all the features, fixes, and other functionality included in the latest dbt Core version! If you have selected the **Compatible** release track, you will have access in the next monthly **Compatible** release after the dbt Core v1.11 final release.

We continue to recommend explicitly installing both `dbt-core` and `dbt-<youradapter>`. This may become required for a future version of dbt. For example:

```sql
python3 -m pip install dbt-core dbt-snowflake
```

## New and changed features and functionality[​](#new-and-changed-features-and-functionality "Direct link to New and changed features and functionality")

New features and functionality available in dbt Core v1.11

### User-defined functions (UDFs)[​](#user-defined-functions-udfs "Direct link to User-defined functions (UDFs)")

dbt Core v1.11 introduces support for user-defined functions (UDFs), which enable you to define and register custom functions in your warehouse. Like macros, UDFs promote code reuse, but they are objects in the warehouse so you can reuse the same logic in tools outside dbt.

Key features include:

* **Define UDFs as first-class dbt resources**: Create UDF files in a `functions/` directory with corresponding YAML configuration.
* **Execution**: Create, update, and rename UDFs as part of DAG execution using `dbt build --select "resource_type:function"`
* **DAG integration**: When executing `dbt build`, UDFs are built before models that reference them, ensuring proper dependency management.
* **New `function()` macro**: Reference UDFs in your models using the `{{ function('function_name') }}` Jinja macro.

Read more about UDFs, including prerequisites and how to define and use them in the [UDF documentation](https://docs.getdbt.com/docs/build/udfs.md).

### Managing changes to legacy behaviors[​](#managing-changes-to-legacy-behaviors "Direct link to Managing changes to legacy behaviors")

dbt Core v1.11 introduces new flags for [managing changes to legacy behaviors](https://docs.getdbt.com/reference/global-configs/behavior-changes.md). You may opt into recently introduced changes (disabled by default), or opt out of mature changes (enabled by default), by setting `True` / `False` values, respectively, for `flags` in `dbt_project.yml`.

You can read more about each of these behavior changes in the following links:

* (Introduced, disabled by default) [`require_unique_project_resource_names`](https://docs.getdbt.com/reference/global-configs/behavior-changes.md#unique-project-resource-names). This flag is set to `False` by default. With this setting, if two unversioned resources in the same package share the same name, dbt continues to run and raises a [`DuplicateNameDistinctNodeTypesDeprecation`](https://docs.getdbt.com/reference/deprecations.md#duplicatenamedistinctnodetypesdeprecation) warning. When set to `True`, dbt raises a `DuplicateResourceNameError` error.

* (Introduced, disabled by default) [`require_ref_searches_node_package_before_root`](https://docs.getdbt.com/reference/global-configs/behavior-changes.md#package-ref-search-order). This flag is set to `False` by default. With this setting, when dbt resolves a `ref()` in a package model, it searches for the referenced model in the root project *first*, then in the package where the model is defined. When set to `True`, dbt searches the package where the model is defined *before* searching the root project.

### Deprecation warnings enabled by default[​](#deprecation-warnings-enabled-by-default "Direct link to Deprecation warnings enabled by default")

Deprecation warnings from JSON schema validation are now enabled by default when validating your YAML configuration files (such as `schema.yml` and `dbt_project.yml`) for projects running using the Snowflake, Databricks, BigQuery, and Redshift adapters.

These warnings help you proactively identify and update deprecated configurations (such as misspelled config keys, deprecated properties, or incorrect data types).

You'll see the following deprecation warnings by default:

* [CustomKeyInConfigDeprecation](https://docs.getdbt.com/reference/deprecations.md#customkeyinconfigdeprecation)
* [CustomKeyInObjectDeprecation](https://docs.getdbt.com/reference/deprecations.md#customkeyinobjectdeprecation)
* [CustomTopLevelKeyDeprecation](https://docs.getdbt.com/reference/deprecations.md#customtoplevelkeydeprecation)
* [MissingPlusPrefixDeprecation](https://docs.getdbt.com/reference/deprecations.md#missingplusprefixdeprecation)
* [SourceOverrideDeprecation](https://docs.getdbt.com/reference/deprecations.md#sourceoverridedeprecation)

Each deprecation type can be silenced using the [warn-error-options](https://docs.getdbt.com/reference/global-configs/warnings.md#configuration) project configuration. For example, to silence all of the above deprecations within `dbt_project.yml`:

dbt\_project.yml

```yml

flags:
  warn_error_options:
    silence:
      - CustomTopLevelKeyDeprecation
      - CustomKeyInConfigDeprecation
      - CustomKeyInObjectDeprecation
      - MissingPlusPrefixDeprecation
      - SourceOverrideDeprecation
```

Alternatively, the `--warn-error-options` flag can be used to silence specific deprecations from the command line:

```sh
dbt parse --warn-error-options '{"silence": ["CustomTopLevelKeyDeprecation", "CustomKeyInConfigDeprecation", "CustomKeyInObjectDeprecation", "MissingPlusPrefixDeprecation", "SourceOverrideDeprecation"]}'
```

To silence *all* deprecation warnings within `dbt_project.yml`:

dbt\_project.yml

```yml

flags:
  warn_error_options:
    silence:
      - Deprecations
```

Similarly, all deprecation warnings can be silenced via the `--warn-error-options` command line flag:

```sh
dbt parse --warn-error-options '{"silence": ["Deprecations"]}'
```

## Adapter-specific features and functionalities[​](#adapter-specific-features-and-functionalities "Direct link to Adapter-specific features and functionalities")

### Snowflake[​](#snowflake "Direct link to Snowflake")

* The Snowflake adapter supports basic table materialization on Iceberg tables registered in a Glue catalog through a [catalog-linked database](https://docs.snowflake.com/en/user-guide/tables-iceberg-catalog-linked-database#label-catalog-linked-db-create). For more information, see [Glue Data Catalog](https://docs.getdbt.com/docs/mesh/iceberg/snowflake-iceberg-support.md#external-catalogs).
* The `cluster_by` configuration is supported in dynamic tables. For more information, see [Dynamic table clustering](https://docs.getdbt.com/reference/resource-configs/snowflake-configs.md#dynamic-table-clustering).
* The `immutable_where` configuration is supported in dynamic tables. For more information, see [Snowflake configurations](https://docs.getdbt.com/reference/resource-configs/snowflake-configs.md#immutable-where).

### BigQuery[​](#bigquery "Direct link to BigQuery")

* To improve performance, dbt can issue a single batch query when calculating source freshness through metadata, instead of executing one query per source. To enable this feature, set [bigquery\_use\_batch\_source\_freshness](https://docs.getdbt.com/reference/global-configs/bigquery-changes.md#the-bigquery_use_batch_source_freshness-flag) to `True`.

### Redshift[​](#redshift "Direct link to Redshift")

* The [`redshift_skip_autocommit_transaction_statements`](https://docs.getdbt.com/reference/global-configs/redshift-changes.md#the-redshift_skip_autocommit_transaction_statements-flag) flag is now `True` by default. When `autocommit=True` (the default since dbt-redshift 1.5), dbt now skips sending unnecessary `BEGIN`/`COMMIT`/`ROLLBACK` statements, improving performance by reducing round trips to Redshift. To preserve the legacy behavior, set the flag to `False`.

### Spark[​](#spark "Direct link to Spark")

* New profile configurations have been added to enhance [retry handling for PyHive connections](https://docs.getdbt.com/reference/resource-configs/spark-configs.md#retry-handling-for-pyhive-connections):

  <!-- -->

  * `poll_interval`: Controls how frequently the adapter polls the Thrift server to check if an async query has completed.
  * `query_timeout`: Adds an overall timeout (in seconds) for query execution. If a query exceeds the set duration during polling, it raises a `DbtRuntimeError`. This helps prevent indefinitely hanging queries.
  * `query_retries`: Handles connection loss during query polling by automatically retrying.

## Quick hits[​](#quick-hits "Direct link to Quick hits")

You will find these quick hits in dbt Core v1.11:

* The `dbt ls` command can now write out nested keys. This makes it easier to debug and troubleshoot your project. Example: `dbt ls --output json --output-keys config.materialized`
* Manifest metadata now includes `run_started_at`, providing better tracking of when dbt runs were initiated.
* When a model is disabled, unit tests for that model are automatically disabled as well.
* You can use the new [`config.meta_get()`](https://docs.getdbt.com/reference/dbt-jinja-functions/config.md#configmeta_get) and [`config.meta_require()`](https://docs.getdbt.com/reference/dbt-jinja-functions/config.md#configmeta_require) functions to access custom configurations stored under `meta`. These functions have been backported to dbt Core v1.10.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
