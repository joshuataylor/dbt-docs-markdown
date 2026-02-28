# Upgrading to v1.12

Beta coming soon

dbt Core v1.12 is not yet available in beta. We will update this guide when it becomes available.

## Resources[​](#resources "Direct link to Resources")

* dbt Core v1.12 changelog (coming soon)
* [dbt Core CLI Installation guide](https://docs.getdbt.com/docs/core/installation-overview.md)
* [Cloud upgrade guide](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-version-in-cloud.md#release-tracks)

## What to know before upgrading[​](#what-to-know-before-upgrading "Direct link to What to know before upgrading")

dbt Labs is committed to providing backward compatibility for all versions 1.x. Any behavior changes will be accompanied by a [behavior change flag](https://docs.getdbt.com/reference/global-configs/behavior-changes.md#behavior-change-flags) to provide a migration window for existing projects. If you encounter an error upon upgrading, please let us know by [opening an issue](https://github.com/dbt-labs/dbt-core/issues/new).

dbt provides the functionality from new versions of dbt Core via [release tracks](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md) with automatic upgrades. If you have selected the **Latest** release track in dbt, you already have access to all the features, fixes, and other functionality included in the latest dbt Core version! If you have selected the **Compatible** release track, you will have access to the next monthly **Compatible** release after the dbt Core v1.12 final release.

We continue to recommend explicitly installing both `dbt-core` and `dbt-<youradapter>`. This may become required for a future version of dbt. For example:

```sql
python3 -m pip install dbt-core dbt-snowflake
```

## New and changed features and functionality[​](#new-and-changed-features-and-functionality "Direct link to New and changed features and functionality")

**Coming soon**

### Managing changes to legacy behaviors[​](#managing-changes-to-legacy-behaviors "Direct link to Managing changes to legacy behaviors")

dbt Core v1.12 introduces new flags for [managing changes to legacy behaviors](https://docs.getdbt.com/reference/global-configs/behavior-changes.md). You may opt into recently introduced changes (disabled by default), or opt out of mature changes (enabled by default), by setting `True` / `False` values, respectively, for `flags` in `dbt_project.yml`.

You can read more about each of these behavior changes in the following links:

* (Introduced, disabled by default) [`require_valid_schema_from_generate_schema_name`](https://docs.getdbt.com/reference/global-configs/behavior-changes.md#valid-schema-from-generate_schema_name). This flag is set to `False` by default. With this setting, dbt raises the [`GenerateSchemaNameNullValueDeprecation`](https://docs.getdbt.com/reference/deprecations.md#generateschemanamenullvaluedeprecation) warning when a custom `generate_schema_name` macro returns a `null` value. When set to `True`, dbt enforces stricter validation and raises a parsing error instead of a warning.

## Adapter-specific features and functionalities[​](#adapter-specific-features-and-functionalities "Direct link to Adapter-specific features and functionalities")

### BigQuery[​](#bigquery "Direct link to BigQuery")

* You can configure BigQuery job link logging with `job_link_info_level_log`. By default, dbt logs job links at the debug level. To log job links at the information level, set `job_link_info_level_log: true` in your BigQuery profile. This makes job links visible in dbt logs for easier access to the BigQuery console. For more information, see [BigQuery setup](https://docs.getdbt.com/docs/core/connect-data-platform/bigquery-setup.md#job_link_info_level_log).

### Redshift[​](#redshift "Direct link to Redshift")

* Added support for the `query_group` session parameter, allowing dbt to tag queries for Redshift Workload Manager routing and query logging. When configured in a profile, dbt sets `query_group` when opening a connection and the value applies for the duration of that session. You can also configure `query_group` at the model level to temporarily override the default value for a specific model, and dbt reverts the value at the end of model materialization. For more information, see [Redshift configurations](https://docs.getdbt.com/reference/resource-configs/redshift-configs.md#session-configuration).

## Quick hits[​](#quick-hits "Direct link to Quick hits")

**Coming soon**

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
