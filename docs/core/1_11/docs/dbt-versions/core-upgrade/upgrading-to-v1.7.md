# Upgrading to v1.7

## Resources[​](#resources "Direct link to Resources")

* [Changelog](https://github.com/dbt-labs/dbt-core/blob/1.7.latest/CHANGELOG.md)
* [dbt Core CLI Installation guide](https://docs.getdbt.com/docs/local/install-dbt.md)
* [Cloud upgrade guide](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md)
* [Release schedule](https://github.com/dbt-labs/dbt-core/issues/8260)

## What to know before upgrading[​](#what-to-know-before-upgrading "Direct link to What to know before upgrading")

dbt Labs is committed to providing backward compatibility for all versions 1.x, with the exception of any changes explicitly mentioned below. If you encounter an error upon upgrading, please let us know by [opening an issue](https://github.com/dbt-labs/dbt-core/issues/new).

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

### Behavior changes[​](#behavior-changes "Direct link to Behavior changes")

dbt Core v1.7 expands the amount of sources you can configure freshness for. Previously, freshness was limited to sources with a `loaded_at_field`; now, freshness can be generated from warehouse metadata tables when available.

As part of this change, the `loaded_at_field` is no longer required to generate source freshness. If a source has a `freshness:` block, dbt will attempt to calculate freshness for that source:

* If a `loaded_at_field` is provided, dbt will calculate freshness via a select query (previous behavior).
* If a `loaded_at_field` is *not* provided, dbt will calculate freshness via warehouse metadata tables when possible (new behavior).

This is a relatively small behavior change, but worth calling out in case you notice that dbt is calculating freshness for *more* sources than before. To exclude a source from freshness calculations, explicitly set `freshness: null`.

Beginning with v1.7, running [`dbt deps`](https://docs.getdbt.com/reference/commands/deps.md) creates or updates the `package-lock.yml` file in the *project\_root* where `packages.yml` is recorded. The `package-lock.yml` file contains a record of all packages installed and, if subsequent `dbt deps` runs contain no updated packages in `dependencies.yml` or `packages.yml`, dbt-core installs from `package-lock.yml`.

To retain the behavior prior to v1.7, there are two main options:

1. Use `dbt deps --upgrade` everywhere `dbt deps` was used previously.
2. Add `package-lock.yml` to your `.gitignore` file.

## New and changed features and functionality[​](#new-and-changed-features-and-functionality "Direct link to New and changed features and functionality")

* [`dbt docs generate`](https://docs.getdbt.com/reference/commands/cmd-docs.md) now supports `--select` to generate [catalog metadata](https://docs.getdbt.com/reference/artifacts/catalog-json.md) for a subset of your project.
* [Source freshness](https://docs.getdbt.com/docs/deploy/source-freshness.md) can now be generated from warehouse metadata tables.

### MetricFlow enhancements[​](#metricflow-enhancements "Direct link to MetricFlow enhancements")

* Automatically create metrics on measures with [`create_metric: true`](https://docs.getdbt.com/docs/build/semantic-models.md).
* Optional [`label`](https://docs.getdbt.com/docs/build/semantic-models.md) in semantic\_models, measures, dimensions, and entities.
* New configurations for semantic models - [enable/disable](https://docs.getdbt.com/reference/resource-configs/enabled.md), [group](https://docs.getdbt.com/reference/resource-configs/group.md), and [meta](https://docs.getdbt.com/reference/resource-configs/meta.md).
* Support `fill_nulls_with` and `join_to_timespine` for metric nodes.
* `saved_queries` extends governance beyond the semantic objects to their consumption.

### For consumers of dbt artifacts (metadata)[​](#for-consumers-of-dbt-artifacts-metadata "Direct link to For consumers of dbt artifacts (metadata)")

* The [manifest](https://docs.getdbt.com/reference/artifacts/manifest-json.md) schema version has been updated to v11.

* The [run\_results](https://docs.getdbt.com/reference/artifacts/run-results-json.md) schema version has been updated to v5.

* There are a few specific changes to the [catalog.json](https://docs.getdbt.com/reference/artifacts/catalog-json.md):

  <!-- -->

  * Added [node attributes](https://docs.getdbt.com/reference/artifacts/run-results-json.md) related to compilation (`compiled`, `compiled_code`, `relation_name`) to the `catalog.json`.
  * The nodes dictionary in the `catalog.json` can now be "partial" if `dbt docs generate` is run with a selector.

### Model governance[​](#model-governance "Direct link to Model governance")

dbt Core v1.5 introduced model governance which we're continuing to refine. v1.7 includes these additional features and functionality:

* **[Breaking change detection](https://docs.getdbt.com/reference/resource-properties/versions.md#detecting-breaking-changes) for models with contracts enforced:** When dbt detects a breaking change to a model with an enforced contract during state comparison, it will now raise an error for versioned models and a warning for models that are not versioned.
* **[Set `access` as a config](https://docs.getdbt.com/reference/resource-configs/access.md):** You can now set a model's `access` within config blocks in the model's SQL file or in the project YAML file (`dbt_project.yml`) for an entire subfolder at once.
* **[Type aliasing for model contracts](https://docs.getdbt.com/reference/resource-configs/contract.md):** dbt will use each adapter's built-in type aliasing for user-provided data types—meaning you can now write `string` always, and dbt will translate to `text` on Postgres/Redshift. This is "on" by default, but you can opt-out.
* **[Raise warning for numeric types](https://docs.getdbt.com/reference/resource-configs/contract.md):** Because of issues when putting `numeric` in model contracts without considering that default values such as `numeric(38,0)` might round decimals accordingly. dbt will now warn you if it finds a numeric type without specified precision/scale.

### dbt clean[​](#dbt-clean "Direct link to dbt clean")

[dbt clean](https://docs.getdbt.com/reference/commands/clean.md) only cleans paths within the current working directory. The `--no-clean-project-files-only` flag will delete all paths specified in the `clean-targets` section of `dbt_project.yml`, even if they're outside the dbt project.

Supported flags:

* `--clean-project-files-only` (default)
* `--no-clean-project-files-only`

### Additional attributes in run\_results.json[​](#additional-attributes-in-run_resultsjson "Direct link to Additional attributes in run_results.json")

The run\_results.json now includes three attributes related to the `applied` state that complement `unique_id`:

* `compiled`: Boolean entry of the node compilation status (`False` after parsing, but `True` after compiling).
* `compiled_code`: Rendered string of the code that was compiled (empty after parsing, but full string after compiling).
* `relation_name`: The fully-qualified name of the object that was (or will be) created/updated within the database.

### Deprecated functionality[​](#deprecated-functionality "Direct link to Deprecated functionality")

The ability for installed packages to override built-in materializations without explicit opt-in from the user is being deprecated.

* Overriding a built-in materialization from an installed package raises a deprecation warning.

* Using a custom materialization from an installed package does not raise a deprecation warning.

* Using a built-in materialization package override from the root project via a wrapping materialization is still supported. For example:

  ```text
  {% materialization view, default %}
  {{ return(my_cool_package.materialization_view_default()) }}
  {% endmaterialization %}
  ```

### Quick hits[​](#quick-hits "Direct link to Quick hits")

With these quick hits, you can now:

* Configure a [`delimiter`](https://docs.getdbt.com/reference/resource-configs/delimiter.md) for a seed file.
* Use packages with the same git repo and unique subdirectory.
* Access the `date_spine` macro directly from dbt-core (moved over from dbt-utils).
* Syntax for `DBT_ENV_SECRET_` has changed to `DBT_ENV_SECRET` and no longer requires the closing underscore.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
