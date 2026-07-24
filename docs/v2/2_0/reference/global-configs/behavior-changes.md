# About behavior changes

A behavior change is a deliberate update to dbt where the same project code and commands produce a different result than before—for example, a new validation error, a changed macro signature, or a breaking change to artifacts or structured logs. It is not a bug fix, a new warning, or a non-breaking addition.

dbt gates these changes behind behavior change flags, so you control when to adopt the new behavior.

The following are examples of behavior changes:

* dbt begins raising a validation *error* that it didn't previously.
* dbt changes the signature of a built-in macro. Your project has a custom reimplementation of that macro. This could lead to errors, because your custom reimplementation will be passed arguments it cannot accept.
* A dbt adapter renames or removes a method that was previously available on the `{{ adapter }}` object in the dbt-Jinja context.

The following are *not* behavior changes:

* Fixing a bug where the previous behavior was defective, undesirable, or undocumented.
* dbt begins raising a *warning* that it didn't previously.
* dbt updates the language of human-friendly messages in log events.

## Behavior change flags[​](#behavior-change-flags "Direct link to Behavior change flags")

These flags *must* be set in the `flags` dictionary in `dbt_project.yml`. They configure behaviors closely tied to project code, which means they should be defined in version control and modified through pull or merge requests, with the same testing and peer review.

### Flag lifecycle[​](#flag-lifecycle "Direct link to Flag lifecycle")

Behavior change flags go through three phases of development:

1. **Introduced (disabled by default):** dbt adds logic to support both 'old' and 'new' behaviors. The 'new' behavior is gated behind a flag, disabled by default, preserving the old behavior.
2. **Mature (enabled by default):** The default value of the flag is switched to the new behavior by default. You can still preserve the old behavior, but you may see deprecation warnings.
3. **Removed (generally enabled):** The old behavior is removed from the dbt codebase(s). Most flags are supported indefinitely, but there is no committement to supporting them forever. If a flag is removed, there will be significant advanced warning.

### Introduced in dbt Core v1.x[​](#introduced-in-dbt-core-v1x "Direct link to Introduced in dbt Core v1.x")

This table outlines which month of the **Latest** release track in dbt and which version of dbt Core contains the behavior change's introduction (disabled by default) or maturity (enabled by default).

| Flag                                                                                                                                                                                                        | dbt **Latest**: Intro | dbt **Latest**: Maturity | dbt Core: Intro | dbt Core: Maturity | dbt Core: Removed |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------- | ------------------------ | --------------- | ------------------ | ----------------- |
| [require\_explicit\_package\_overrides\_for\_builtin\_materializations](https://docs.getdbt.com/reference/global-configs/behavior-flags/require_explicit_package_overrides_for_builtin_materializations.md) | 2024.04               | 2024.06                  | 1.6.14, 1.7.14  | 1.8.0              | 2.0               |
| [require\_resource\_names\_without\_spaces](https://docs.getdbt.com/reference/global-configs/behavior-flags/require_resource_names_without_spaces.md)                                                       | 2024.05               | 2025.05                  | 1.8.0           | 1.10.0             | 2.0               |
| [source\_freshness\_run\_project\_hooks](https://docs.getdbt.com/reference/global-configs/behavior-flags/source_freshness_run_project_hooks.md)                                                             | 2024.03               | 2025.05                  | 1.8.0           | 1.10.0             | 2.0               |
| [skip\_nodes\_if\_on\_run\_start\_fails](https://docs.getdbt.com/reference/global-configs/behavior-flags/skip_nodes_if_on_run_start_fails.md)                                                               | 2024.10               | -                        | 1.9.0           | 1.12.0             | 2.0               |
| [state\_modified\_compare\_more\_unrendered\_values](https://docs.getdbt.com/reference/global-configs/behavior-flags/state_modified_compare_more_unrendered_values.md)                                      | 2024.10               | -                        | 1.9.0           | 1.12.0             | 2.0               |
| [require\_yaml\_configuration\_for\_mf\_time\_spines](https://docs.getdbt.com/reference/global-configs/behavior-flags/require_yaml_configuration_for_mf_time_spines.md)                                     | 2024.10               | -                        | 1.9.0           | 1.12.0             | 2.0               |
| [require\_batched\_execution\_for\_custom\_microbatch\_strategy](https://docs.getdbt.com/reference/global-configs/behavior-flags/require_batched_execution_for_custom_microbatch_strategy.md)               | 2024.11               | -                        | 1.9.0           | 1.12.0             | 2.0               |
| [require\_nested\_cumulative\_type\_params](https://docs.getdbt.com/reference/global-configs/behavior-flags/require_nested_cumulative_type_params.md)                                                       | 2024.11               | -                        | 1.9.0           | 1.12.0             | -                 |
| [enable\_truthy\_nulls\_equals\_macro](https://docs.getdbt.com/reference/global-configs/behavior-flags/enable_truthy_nulls_equals_macro.md)                                                                 | 2025.02               | -                        | 1.9.0           | -                  | -                 |
| [validate\_macro\_args](https://docs.getdbt.com/reference/global-configs/behavior-flags/validate_macro_args.md)                                                                                             | 2025.03               | -                        | 1.10.0          | 1.12.0             | -                 |
| [require\_all\_warnings\_handled\_by\_warn\_error](https://docs.getdbt.com/reference/global-configs/behavior-flags/require_all_warnings_handled_by_warn_error.md)                                           | 2025.06               | -                        | 1.10.0          | 1.12.0             | -                 |
| [require\_generic\_test\_arguments\_property](https://docs.getdbt.com/reference/global-configs/behavior-flags/require_generic_test_arguments_property.md)                                                   | 2025.07               | 2025.08                  | 1.10.5          | 1.10.8             | -                 |
| [require\_unique\_project\_resource\_names](https://docs.getdbt.com/reference/global-configs/behavior-flags/require_unique_project_resource_names.md)                                                       | 2025.12               | -                        | 1.11.0          | -                  | -                 |
| [require\_ref\_searches\_node\_package\_before\_root](https://docs.getdbt.com/reference/global-configs/behavior-flags/require_ref_searches_node_package_before_root.md)                                     | 2025.12               | -                        | 1.11.0          | -                  | -                 |
| [require\_valid\_schema\_from\_generate\_schema\_name](https://docs.getdbt.com/reference/global-configs/behavior-flags/require_valid_schema_from_generate_schema_name.md)                                   | 2026.1                | -                        | 1.12.0a1        | -                  | -                 |
| [require\_sql\_header\_in\_test\_configs](https://docs.getdbt.com/reference/global-configs/behavior-flags/require_sql_header_in_test_configs.md)                                                            | 2026.3                | -                        | 1.12.0          | -                  | -                 |
| [require\_corrected\_analysis\_fqns](https://docs.getdbt.com/reference/global-configs/behavior-flags/require_corrected_analysis_fqns.md)                                                                    | 2026.3                | -                        | 1.12.0          | -                  | -                 |
| [require\_source\_and\_semantic\_model\_names\_without\_spaces](https://docs.getdbt.com/reference/global-configs/behavior-flags/require_source_and_semantic_model_names_without_spaces.md)                  | 2026.4                | -                        | 1.12.0          | -                  | -                 |
| [allow\_jinja\_file\_extensions](https://docs.getdbt.com/reference/global-configs/behavior-flags/allow_jinja_file_extensions.md)                                                                            | 2026.5                | -                        | 1.12.0          | -                  | -                 |
| [latest\_version\_pointer\_enabled\_by\_default](https://docs.getdbt.com/reference/global-configs/behavior-flags/latest_version_pointer_enabled_by_default.md)                                              | 2026.5                | -                        | 1.12.0          | -                  | -                 |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Flags reaching maturity[​](#flags-reaching-maturity "Direct link to Flags reaching maturity")

Several behavior change flags on the dbt platform `Latest` release track are planned to reach maturity on September 1, 2026, switching their default values from `false` to `true`. The September 1 date applies only to the dbt platform release tracks. The flags have reached maturity in dbt Core v1.12. For intro dates, refer to the dbt Core behavior changes table.

| Flag                                                                                                                                                                                          | Impact                                                                 |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| [skip\_nodes\_if\_on\_run\_start\_fails](https://docs.getdbt.com/reference/global-configs/behavior-flags/skip_nodes_if_on_run_start_fails.md)                                                 | Can stop build                                                         |
| [require\_nested\_cumulative\_type\_params](https://docs.getdbt.com/reference/global-configs/behavior-flags/require_nested_cumulative_type_params.md)                                         | Can stop build (parse error)                                           |
| [require\_all\_warnings\_handled\_by\_warn\_error](https://docs.getdbt.com/reference/global-configs/behavior-flags/require_all_warnings_handled_by_warn_error.md)                             | Can stop build (when `--warn-error` is set)                            |
| [require\_batched\_execution\_for\_custom\_microbatch\_strategy](https://docs.getdbt.com/reference/global-configs/behavior-flags/require_batched_execution_for_custom_microbatch_strategy.md) | Behavior change for custom microbatch macros                           |
| [state\_modified\_compare\_more\_unrendered\_values](https://docs.getdbt.com/reference/global-configs/behavior-flags/state_modified_compare_more_unrendered_values.md)                        | Selection-set change with potential CI impact                          |
| [require\_yaml\_configuration\_for\_mf\_time\_spines](https://docs.getdbt.com/reference/global-configs/behavior-flags/require_yaml_configuration_for_mf_time_spines.md)                       | Suppresses a deprecation warning (no functional change)                |
| [validate\_macro\_args](https://docs.getdbt.com/reference/global-configs/behavior-flags/validate_macro_args.md)                                                                               | New warning for mismatched macro arguments; errors with `--warn-error` |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Introduced in Fusion and Core v2[​](#introduced-in-fusion-and-core-v2 "Direct link to Introduced in Fusion and Core v2")

The following flags are specific to Fusion and have no equivalent in dbt Core. They are configured the same way — in the `flags:` block of `dbt_project.yml`.

| Flag                                     | Adapter  | Default | Introduced                         | Becomes default |
| ---------------------------------------- | -------- | ------- | ---------------------------------- | --------------- |
| use\_catalogs\_v2                        | All      | `false` | Fusion preview\.174 (Apr 22, 2026) | Not yet set     |
| bigquery\_noop\_alter\_relation\_comment | BigQuery | `false` | Fusion preview\.124 (Feb 19, 2026) | Not yet set     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Adapter-specific behavior change flags[​](#adapter-specific-behavior-change-flags "Direct link to Adapter-specific behavior change flags")

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
| [bigquery\_use\_standard\_sql\_for\_partitions](https://docs.getdbt.com/reference/global-configs/bigquery-changes.md#the-bigquery_use_standard_sql_for_partitions-flag)                     | BigQuery 1.12.0    | 1.12.0                | -                 |
| [snowflake\_default\_transient\_dynamic\_tables](https://docs.getdbt.com/reference/global-configs/snowflake-changes.md#the-snowflake_default_transient_dynamic_tables-flag)                 | Snowflake 1.12.0   | -                     | -                 |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## FAQs[​](#faqs "Direct link to FAQs")

 How do I implement behavior change flags in my project?

The following example displays the current flags and their current default values in the latest dbt and dbt Core versions. To opt out of a specific behavior change, set the value of the flag to `false` in `dbt_project.yml`. You will continue to see warnings for legacy behaviors you've opted out of, until you either:

* Resolve the issue (by switching the flag to `true`)
* Silence the warnings using the `warn_error_options.silence` flag

dbt\_project.yml

```yml
flags:
  require_explicit_package_overrides_for_builtin_materializations: true
  require_resource_names_without_spaces: true
  source_freshness_run_project_hooks: true
  skip_nodes_if_on_run_start_fails: false  # true in dbt Core v1.12
  state_modified_compare_more_unrendered_values: false  # true in dbt Core v1.12
  require_yaml_configuration_for_mf_time_spines: false  # true in dbt Core v1.12
  require_batched_execution_for_custom_microbatch_strategy: false  # true in dbt Core v1.12
  require_nested_cumulative_type_params: false  # true in dbt Core v1.12
  validate_macro_args: false  # true in dbt Core v1.12
  require_all_warnings_handled_by_warn_error: false  # true in dbt Core v1.12
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

 What does it mean if there's no maturity date?

When a maturity date has not yet been set (shown as -), we have not yet determined the exact date when the flag's default value will change. Affected users will see deprecation warnings in the meantime, and they will receive emails providing advance warning ahead of the maturity date. In the meantime, if you are seeing a deprecation warning, you can either:

* Migrate your project to support the new behavior, and then set the flag to true to stop seeing the warnings.
* Explicitly set the flag to `false`. You will continue to see warnings, and you will retain the legacy behavior even after the maturity date (when the default value changes).

 How do behavior change flags related to other changes?

Since behavior change flags are different from other dbt changes, it's important to understand the difference:

* [Deprecation warnings](https://docs.getdbt.com/reference/deprecations.md) — Features in your project code that will stop working (behavior flags often control when these become errors)
* [Deprecated CLI flags](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md#deprecated-flags) — Command-line flags being removed in dbt Fusion

See the [Changes overview](https://docs.getdbt.com/reference/changes-overview.md) for a quick comparison.

If you're upgrading to [dbt Fusion](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md) or [dbt Core 2.0](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md), a subset of behavior change flags are removed and their new behavior is always enabled.
