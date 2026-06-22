# Upgrading to v1.8

## Resources[​](#resources "Direct link to Resources")

* [Changelog](https://github.com/dbt-labs/dbt-core/blob/1.8.latest/CHANGELOG.md)
* [dbt Core CLI Installation guide](https://docs.getdbt.com/docs/local/install-dbt.md)
* [Cloud upgrade guide](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md)

## What to know before upgrading[​](#what-to-know-before-upgrading "Direct link to What to know before upgrading")

dbt Labs is committed to providing backward compatibility for all versions 1.x, except for any changes explicitly mentioned on this page. If you encounter an error upon upgrading, please let us know by [opening an issue](https://github.com/dbt-labs/dbt-core/issues/new).

## Release tracks[​](#release-tracks "Direct link to Release tracks")

Starting in 2024, dbt provides the functionality from new versions of dbt Core via [release tracks](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md) with automatic upgrades. Select a release track in your development, staging, and production [environments](https://docs.getdbt.com/docs/deploy/deploy-environments.md) to access everything in dbt Core v1.8+ and more. To upgrade an environment in the [dbt Admin API](https://docs.getdbt.com/docs/dbt-apis/admin-api.md) or [Terraform](https://registry.terraform.io/providers/dbt-labs/dbtcloud/latest), set `dbt_version` to the string `latest`.

## New and changed features and functionality[​](#new-and-changed-features-and-functionality "Direct link to New and changed features and functionality")

Features and functionality new in dbt v1.8.

<!-- -->

Snowflake column size change

[Snowflake plans to increase](https://docs.snowflake.com/en/release-notes/bcr-bundles/un-bundled/bcr-2118) the default column size for string and binary data types in September 2026. `dbt-snowflake` versions below v1.10.6 may fail to build certain incremental models when this change is deployed.

 Assess impact and required actions

If you're using a `dbt-snowflake` version below v1.10.6 or have not yet migrated to a [release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md) in the dbt platform, your adapter version is incompatible with this change and may fail to build incremental models that meet *both* of the following conditions:

* Contain string columns with collation defined
* Use the `on_schema_change='sync_all_columns'` config

To check whether this change affects your project, run the following [list](https://docs.getdbt.com/reference/commands/list.md) command:

```bash
dbt ls -s config.materialized:incremental,config.on_schema_change:sync_all_columns --resource-type model
```

* If the command returns `No nodes selected!`, no action is required.

* If the command returns one or more models (for example, `Found 1000 models, 644 macros`), you may be impacted if those models have string columns that don't specify a width. In that case, upgrade to a version that includes the fix:

  * **dbt Core**: `dbt-snowflake` v1.10.6 or later. For upgrade instructions, refer to [Upgrade adapters](https://docs.getdbt.com/docs/local/install-dbt.md) in the dbt Core v1 installation instructions.
  * **dbt platform**: Any release track (Latest, Compatible, Extended, or Fallback).
  * **dbt Fusion engine**: v2.0.0-preview\.147 or higher.

  This ensures your incremental models can safely handle schema changes while maintaining required collation settings.

### Unit Tests[​](#unit-tests "Direct link to Unit Tests")

Historically, dbt's test coverage was confined to [“data” tests](https://docs.getdbt.com/docs/build/data-tests.md), assessing the quality of input data or resulting datasets' structure.

In v1.8, we're introducing native support for [unit testing](https://docs.getdbt.com/docs/build/unit-tests.md). Unit tests validate your SQL modeling logic on a small set of static inputs **before** you materialize your full model in production. They support a test-driven development approach, improving both the efficiency of developers and the reliability of code.

Starting from v1.8, when you execute the `dbt test` command, it will run both unit and data tests. Use the [`test_type`](https://docs.getdbt.com/reference/node-selection/methods.md#test_type) method to run only unit or data tests:

```shell

dbt test --select "test_type:unit"           # run all unit tests
dbt test --select "test_type:data"           # run all data tests
```

Unit tests are defined in YML files in your `models/` directory and are currently only supported on SQL models. To distinguish between the two, the `tests:` config has been renamed to `data_tests:`. Both are currently supported for backward compatibility.

#### New `data_tests:` syntax[​](#new-data_tests-syntax "Direct link to new-data_tests-syntax")

The `tests:` syntax is changing to reflect the addition of unit tests. Start migrating your [data test](https://docs.getdbt.com/docs/build/data-tests.md#new-data_tests-syntax) YML to use `data_tests:` after you upgrade to v1.8 to prevent issues in the future.

```yml

models:
  - name: orders
    columns:
      - name: order_id
        data_tests:
          - unique
          - not_null
```

#### The `--empty` flag[​](#the---empty-flag "Direct link to the---empty-flag")

The [`run`](https://docs.getdbt.com/reference/commands/run.md#the-%60--empty%60-flag) and [`build`](https://docs.getdbt.com/reference/commands/build.md#the---empty-flag) commands now support the `--empty` flag for building schema-only dry runs. The `--empty` flag limits the refs and sources to zero rows. dbt will still execute the model SQL against the target data warehouse but will avoid expensive reads of input data. This validates dependencies and ensures your models will build properly.

### dbt-core and adapters are decoupled[​](#dbt-core-and-adapters-are-decoupled "Direct link to dbt-core and adapters are decoupled")

Before v1.8, dbt adapters directly depended on components of `dbt-core`, and `dbt-core` depended on the adapter for execution. This bidirectional dependency made it difficult to develop adapters independently. Starting in dbt Core v1.8, [`dbt-core` and adapters are decoupled](https://github.com/dbt-labs/dbt-adapters/discussions/87), making it easier to maintain and evolve them independent of each other.

For backward compatibility, adapter packages continue to depend on `dbt-core` at install time. Running a dbt project still requires both `dbt-core` and an adapter, and since v1.0, many users have installed both together using [`pip install dbt-<adapter>`](<https://docs.getdbt.com/docs/dbt-versions/core-upgrade/Older versions/upgrading-to-v1.0.md#installation>).

This behavior remains unchanged. For example, `pip install dbt-snowflake` installs the latest versions of both `dbt-core` and `dbt-snowflake`.

### Deprecated functionality[​](#deprecated-functionality "Direct link to Deprecated functionality")

The ability for installed packages to override built-in materializations without explicit opt-in from the user is being deprecated.

* Overriding a built-in materialization from an installed package raises a deprecation warning.

* Using a custom materialization from an installed package does not raise a deprecation warning.

* Using a built-in materialization package override from the root project via a wrapping materialization is still supported. For example:

  ```sql
  {% materialization view, default %}
  {{ return(my_cool_package.materialization_view_default()) }}
  {% endmaterialization %}
  ```

### Managing changes to legacy behaviors[​](#managing-changes-to-legacy-behaviors "Direct link to Managing changes to legacy behaviors")

dbt Core v1.8 has introduced flags for [managing changes to legacy behaviors](https://docs.getdbt.com/reference/global-configs/behavior-changes.md). You may opt into recently introduced changes (disabled by default), or opt out of mature changes (enabled by default), by setting `true` / `false` values, respectively, for `flags` in `dbt_project.yml`.

You can read more about each of these behavior changes in the following links:

* (Mature, enabled by default) [Require explicit package overrides for builtin materializations](https://docs.getdbt.com/reference/global-configs/behavior-flag-maturity.md#require_explicit_package_overrides_for_builtin_materializations)
* (Introduced, disabled by default) [Require resource names without spaces](https://docs.getdbt.com/reference/global-configs/behavior-flag-maturity.md#require_resource_names_without_spaces)
* (Introduced, disabled by default) [Run project hooks (`on-run-*`) in the `dbt source freshness` command](https://docs.getdbt.com/reference/global-configs/behavior-flag-maturity.md#source_freshness_run_project_hooks)

## Quick hits[​](#quick-hits "Direct link to Quick hits")

* Custom defaults of [global config flags](https://docs.getdbt.com/reference/global-configs/about-global-configs.md) should be set in the `flags` dictionary in [`dbt_project.yml`](https://docs.getdbt.com/reference/dbt_project.yml.md), instead of in [`profiles.yml`](https://docs.getdbt.com/docs/local/profiles.yml.md). Support for `profiles.yml` has been deprecated.
* New CLI flag [`--resource-type`/`--exclude-resource-type`](https://docs.getdbt.com/reference/global-configs/resource-type.md) for including/excluding resources from dbt `build`, `run`, and `clone`.
* To improve performance, dbt now issues a single (batch) query when calculating `source freshness` through metadata, instead of executing a query per source.
* Syntax for `DBT_ENV_SECRET_` has changed to `DBT_ENV_SECRET` and no longer requires the closing underscore.
* The `--select` flag now reliably limits `catalog.json` to the selected nodes in all projects, regardless of project size. In earlier versions, node selectors were ignored for projects with 100 or more nodes, and sources were omitted from `catalog.json` entirely when more than 100 nodes were selected.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
