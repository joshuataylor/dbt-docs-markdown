# Behavior changes

Behavior change flags let you control when to adopt new runtime behaviors in dbt. They're configured in your dbt\_project.yml file.

Behavior change flags reaching maturity

Several behavior change flags on the dbt platform **Latest** release track are planned to be enabled by default on June 29, 2026. Refer to [Flags reaching maturity](https://docs.getdbt.com/reference/global-configs/behavior-flag-maturity.md#flags-reaching-maturity) to see which flags are affected, how they may impact your project, and how to opt out before the change takes effect.

How this relates to other changes

Since behavior change flags are different from other dbt changes, it's important to understand the difference:

* [Deprecation warnings](https://docs.getdbt.com/reference/deprecations.md) — Features in your project code that will stop working (behavior flags often control when these become errors)
* [Deprecated CLI flags](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md#deprecated-flags) — Command-line flags being removed in dbt Fusion

See the [Changes overview](https://docs.getdbt.com/reference/changes-overview.md) for a quick comparison.

If you're upgrading to [dbt Fusion](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md) or [dbt Core v2](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md), a subset of behavior change flags are removed and their new behavior is always enabled.

Most flags exist to configure runtime behaviors with multiple valid choices. The right choice may vary based on the environment, user preference, or the specific invocation.

Another category of flags provides existing projects with a migration window for runtime behaviors that are changing in newer releases of dbt. These flags help us achieve a balance between these goals, which can otherwise be in tension, by:

* Providing a better, more sensible, and more consistent default behavior for new users/projects.
* Providing a migration window for existing users/projects — nothing changes overnight without warning.
* Providing maintainability of dbt software. Every fork in behavior requires additional testing & cognitive overhead that slows future development. These flags exist to facilitate migration from "current" to "better," not to stick around forever.

These flags go through three phases of development:

1. **Introduction (disabled by default):** dbt adds logic to support both 'old' and 'new' behaviors. The 'new' behavior is gated behind a flag, disabled by default, preserving the old behavior. For flags still in the introduction phase, refer to [Introduced behavior flags](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md).
2. **Maturity (enabled by default):** The default value of the flag is switched, from `false` to `true`, enabling the new behavior by default. Users can preserve the 'old' behavior and opt out of the 'new' behavior by setting the flag to `false` in their projects. They may see deprecation warnings when they do so. For flags that have already reached maturity, refer to [Mature behavior flags](https://docs.getdbt.com/reference/global-configs/behavior-flag-maturity.md).
3. **Removal (generally enabled):** After marking the flag for deprecation, we remove it along with the 'old' behavior it supported from the dbt codebases. We aim to support most flags indefinitely, but we're not committed to supporting them forever. If we choose to remove a flag, we'll offer significant advance notice. For flags removed in dbt Core v2, refer to [Removed behavior flags](https://docs.getdbt.com/reference/global-configs/behavior-flag-removed.md).

## What is a behavior change?[​](#what-is-a-behavior-change "Direct link to What is a behavior change?")

The same dbt project code and the same dbt commands return one result before the behavior change, and they return a different result after the behavior change.

Examples of behavior changes:

* dbt begins raising a validation *error* that it didn't previously.
* dbt changes the signature of a built-in macro. Your project has a custom reimplementation of that macro. This could lead to errors, because your custom reimplementation will be passed arguments it cannot accept.
* A dbt adapter renames or removes a method that was previously available on the `{{ adapter }}` object in the dbt-Jinja context.
* dbt makes a breaking change to contracted metadata artifacts by deleting a required field, changing the name or type of an existing field, or removing the default value of an existing field ([README](https://github.com/dbt-labs/dbt-core/blob/1.latest/docs/arch/7_Artifacts.md#breaking-changes)).
* dbt removes one of the fields from [structured logs](https://docs.getdbt.com/reference/events-logging.md#structured-logging).

The following are **not** behavior changes:

* Fixing a bug where the previous behavior was defective, undesirable, or undocumented.
* dbt begins raising a *warning* that it didn't previously.
* dbt updates the language of human-friendly messages in log events.
* dbt makes a non-breaking change to contracted metadata artifacts by adding a new field with a default, or deleting a field with a default ([README](https://github.com/dbt-labs/dbt-core/blob/1.latest/docs/arch/7_Artifacts.md#non-breaking-changes)).

The vast majority of changes are not behavior changes. Because introducing these changes does not require any action on the part of users, they are included in continuous releases of dbt and patch releases of dbt Core.

By contrast, behavior change migrations happen slowly, over the course of months, facilitated by behavior change flags. The flags are loosely coupled to the specific dbt runtime version. By setting flags, users have control over opting in (and later opting out) of these changes.

## Behavior change flags[​](#behavior-change-flags "Direct link to Behavior change flags")

These flags *must* be set in the `flags` dictionary in `dbt_project.yml`. They configure behaviors closely tied to project code, which means they should be defined in version control and modified through pull or merge requests, with the same testing and peer review.

The following example displays the current flags and their current default values in the latest dbt and dbt Core versions. To opt out of a specific behavior change, set the values of the flag to `false` in `dbt_project.yml`. You will continue to see warnings for legacy behaviors you've opted out of, until you either:

* Resolve the issue (by switching the flag to `true`)
* Silence the warnings using the `warn_error_options.silence` flag

Here's an example of the available behavior change flags with their default values:

dbt\_project.yml

```yml
flags:
  require_explicit_package_overrides_for_builtin_materializations: true
  require_resource_names_without_spaces: true
  source_freshness_run_project_hooks: true
  skip_nodes_if_on_run_start_fails: false
  state_modified_compare_more_unrendered_values: false
  require_yaml_configuration_for_mf_time_spines: false
  require_batched_execution_for_custom_microbatch_strategy: false
  require_nested_cumulative_type_params: false
  validate_macro_args: false
  require_all_warnings_handled_by_warn_error: false
  require_generic_test_arguments_property: true
  require_unique_project_resource_names: false
  require_ref_searches_node_package_before_root: false
  require_valid_schema_from_generate_schema_name: false
  enable_truthy_nulls_equals_macro: false
  require_sql_header_in_test_configs: false
  require_corrected_analysis_fqns: false
  require_source_and_semantic_model_names_without_spaces: false
  allow_jinja_file_extensions: false
  latest_version_pointer_enabled_by_default: false
```

#### dbt Core behavior changes[​](#dbt-core-behavior-changes "Direct link to dbt Core behavior changes")

This table outlines which month of the **Latest** release track in dbt and which version of dbt Core contains the behavior change's introduction (disabled by default) or maturity (enabled by default).

| Flag                                                                                                                                                                                                                | dbt **Latest**: Intro | dbt **Latest**: Maturity | dbt Core: Intro | dbt Core: Maturity | dbt Core: Removed |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- | ------------------------ | --------------- | ------------------ | ----------------- |
| [require\_explicit\_package\_overrides\_for\_builtin\_materializations](https://docs.getdbt.com/reference/global-configs/behavior-flag-maturity.md#require_explicit_package_overrides_for_builtin_materializations) | 2024.04               | 2024.06                  | 1.6.14, 1.7.14  | 1.8.0              | 2.0               |
| [require\_resource\_names\_without\_spaces](https://docs.getdbt.com/reference/global-configs/behavior-flag-maturity.md#require_resource_names_without_spaces)                                                       | 2024.05               | 2025.05                  | 1.8.0           | 1.10.0             | 2.0               |
| [source\_freshness\_run\_project\_hooks](https://docs.getdbt.com/reference/global-configs/behavior-flag-maturity.md#source_freshness_run_project_hooks)                                                             | 2024.03               | 2025.05                  | 1.8.0           | 1.10.0             | 2.0               |
| [skip\_nodes\_if\_on\_run\_start\_fails](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md#failures-in-on-run-start-hooks)                                                             | 2024.10               | -                        | 1.9.0           | -                  | 2.0               |
| [state\_modified\_compare\_more\_unrendered\_values](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md#source-definitions-for-statemodified)                                           | 2024.10               | -                        | 1.9.0           | -                  | 2.0               |
| [require\_yaml\_configuration\_for\_mf\_time\_spines](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md#metricflow-time-spine-yaml)                                                    | 2024.10               | -                        | 1.9.0           | -                  | 2.0               |
| [require\_batched\_execution\_for\_custom\_microbatch\_strategy](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md#custom-microbatch-strategy)                                         | 2024.11               | -                        | 1.9.0           | -                  | 2.0               |
| [require\_nested\_cumulative\_type\_params](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md#cumulative-metrics)                                                                      | 2024.11               | -                        | 1.9.0           | -                  | -                 |
| [enable\_truthy\_nulls\_equals\_macro](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md#null-safe-equality)                                                                           | 2025.02               | -                        | 1.9.0           | -                  | -                 |
| [validate\_macro\_args](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md#macro-argument-validation)                                                                                   | 2025.03               | -                        | 1.10.0          | -                  | -                 |
| [require\_all\_warnings\_handled\_by\_warn\_error](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md#warn-error-handler-for-all-warnings)                                              | 2025.06               | -                        | 1.10.0          | -                  | -                 |
| [require\_generic\_test\_arguments\_property](https://docs.getdbt.com/reference/global-configs/behavior-flag-maturity.md#require_generic_test_arguments_property)                                                   | 2025.07               | 2025.08                  | 1.10.5          | 1.10.8             | -                 |
| [require\_unique\_project\_resource\_names](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md#unique-project-resource-names)                                                           | 2025.12               | -                        | 1.11.0          | -                  | -                 |
| [require\_ref\_searches\_node\_package\_before\_root](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md#package-ref-search-order)                                                      | 2025.12               | -                        | 1.11.0          | -                  | -                 |
| [require\_valid\_schema\_from\_generate\_schema\_name](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md#valid-schema-from-generate_schema_name)                                       | 2026.1                | -                        | 1.12.0a1        | -                  | -                 |
| [require\_sql\_header\_in\_test\_configs](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md#sql_header-in-data-tests)                                                                  | 2026.3                | -                        | 1.12.0          | -                  | -                 |
| [require\_corrected\_analysis\_fqns](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md#project-level-configuration-for-analyses)                                                       | 2026.3                | -                        | 1.12.0          | -                  | -                 |
| [require\_source\_and\_semantic\_model\_names\_without\_spaces](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md#no-spaces-in-source-and-semantic-model-names)                        | 2026.4                | -                        | 1.12.0          | -                  | -                 |
| [allow\_jinja\_file\_extensions](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md#jinja-file-extensions)                                                                              | 2026.5                | -                        | 1.12.0          | -                  | -                 |
| [latest\_version\_pointer\_enabled\_by\_default](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md#latest-version-pointer-for-versioned-models)                                        | 2026.5                | -                        | 1.12.0          | -                  | -                 |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

When a maturity date has not yet been set (shown as `-`), we have not yet determined the exact date when the flag's default value will change. Affected users will see deprecation warnings in the meantime, and they will receive emails providing advance warning ahead of the maturity date. In the meantime, if you are seeing a deprecation warning, you can either:

* Migrate your project to support the new behavior, and then set the flag to `true` to stop seeing the warnings.
* Explicitly set the flag to `false`. You will continue to see warnings, and you will retain the legacy behavior even after the maturity date (when the default value changes).

#### dbt adapter behavior changes[​](#dbt-adapter-behavior-changes "Direct link to dbt adapter behavior changes")

This table outlines which version of the dbt adapter contains the behavior change's introduction (disabled by default) or maturity (enabled by default).

| Flag                                                                                                                                                                                        | dbt-ADAPTER: Intro | dbt-ADAPTER: Maturity | dbt Core: Removed |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------ | --------------------- | ----------------- |
| [use\_info\_schema\_for\_columns](https://docs.getdbt.com/reference/global-configs/databricks-changes.md#use-information-schema-for-columns)                                                | Databricks 1.9.0   | -                     | 2.0               |
| [use\_user\_folder\_for\_python](https://docs.getdbt.com/reference/global-configs/databricks-changes.md#use-users-folder-for-python-model-notebooks)                                        | Databricks 1.9.0   | -                     | 2.0               |
| [use\_managed\_iceberg](https://docs.getdbt.com/reference/global-configs/databricks-changes.md#use-managed-iceberg)                                                                         | Databricks 1.11.0  | 1.12.0                | -                 |
| [use\_materialization\_v2](https://docs.getdbt.com/reference/global-configs/databricks-changes.md#use-restructured-materializations)                                                        | Databricks 1.10.0  | -                     | -                 |
| [use\_replace\_on\_for\_insert\_overwrite](https://docs.getdbt.com/reference/global-configs/databricks-changes.md#use-replace-on-for-insert_overwrite-strategy)                             | Databricks 1.11.0  | 1.11.0                | -                 |
| [use\_describe\_as\_json\_for\_relation\_metadata](https://docs.getdbt.com/reference/global-configs/databricks-changes.md#use-describe-as-json-for-relation-metadata)                       | Databricks 1.12.0  | -                     | -                 |
| [redshift\_skip\_autocommit\_transaction\_statements](https://docs.getdbt.com/reference/global-configs/redshift-changes.md#redshift_skip_autocommit_transaction_statements-flag)            | Redshift 1.12.0    | -                     | -                 |
| [bigquery\_use\_batch\_source\_freshness](https://docs.getdbt.com/reference/global-configs/bigquery-changes.md#bigquery-use-batch-source-freshness)                                         | BigQuery 1.11.0rc2 | -                     | -                 |
| [bigquery\_reject\_wildcard\_metadata\_source\_freshness](https://docs.getdbt.com/reference/global-configs/bigquery-changes.md#the-bigquery_reject_wildcard_metadata_source_freshness-flag) | BigQuery 1.12.0    | -                     | -                 |
| [snowflake\_default\_transient\_dynamic\_tables](https://docs.getdbt.com/reference/global-configs/snowflake-changes.md#the-snowflake_default_transient_dynamic_tables-flag)                 | Snowflake 1.12.0   | -                     | -                 |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
