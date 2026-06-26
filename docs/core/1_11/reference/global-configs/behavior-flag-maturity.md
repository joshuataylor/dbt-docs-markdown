# Mature behavior flags

When a behavior change flag reaches maturity, its default value switches from `false` to `true`. This page covers two categories of flags:

* **[Mature flags](#mature-flags)**: Flags that have already reached maturity and are enabled by default
* **[Flags reaching maturity](#flags-reaching-maturity)**: Flags planned to reach maturity on the dbt platform **Latest** release track, where the default will switch to `true`

For each flag, this page describes what the change means for your project and how to preserve the previous behavior. For flags still in the introduction phase, refer to [Behavior flag introduction](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md).

## Mature flags[​](#mature-flags "Direct link to Mature flags")

The following flags have reached maturity:

| Flag                                                                                                                                  | dbt **Latest**: Maturity | dbt Core: Maturity |
| ------------------------------------------------------------------------------------------------------------------------------------- | ------------------------ | ------------------ |
| [`require_explicit_package_overrides_for_builtin_materializations`](#require_explicit_package_overrides_for_builtin_materializations) | 2024.06                  | v1.8.0             |
| [`require_resource_names_without_spaces`](#require_resource_names_without_spaces)                                                     | 2025.05                  | v1.10.0            |
| [`source_freshness_run_project_hooks`](#source_freshness_run_project_hooks)                                                           | 2025.05                  | v1.10.0            |
| [`require_generic_test_arguments_property`](#require_generic_test_arguments_property)                                                 | 2025.08                  | v1.10.8            |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

To opt out of mature flags and preserve the previous behavior, set them explicitly to `false` in your `dbt_project.yml`:

dbt\_project.yml

```yml
flags:
  require_explicit_package_overrides_for_builtin_materializations: false
  require_resource_names_without_spaces: false
  source_freshness_run_project_hooks: false
  require_generic_test_arguments_property: false
```

### `require_explicit_package_overrides_for_builtin_materializations`[​](#require_explicit_package_overrides_for_builtin_materializations "Direct link to require_explicit_package_overrides_for_builtin_materializations")

Installed packages can no longer override built-in materializations without your explicit opt-in. A materialization defined in a package that matches the name of a built-in materialization is no longer included in the search and resolution order. Unlike macros, materializations don't use the `search_order` defined in the project `dispatch` config.

The built-in materializations are `'view'`, `'table'`, `'incremental'`, `'materialized_view'` for models as well as `'test'`, `'unit'`, `'snapshot'`, `'seed'`, and `'clone'`.

You can still explicitly override built-in materializations by reimplementing the built-in materialization in your root project and wrapping the package implementation.

macros/materialization\_view\.sql

```sql
{% materialization view, snowflake %}
  {{ return(my_installed_package_name.materialization_view_snowflake()) }}
{% endmaterialization %}
```

In the future, we may extend the project-level [`dispatch` configuration](https://docs.getdbt.com/reference/project-configs/dispatch-config.md) to support a list of authorized packages for overriding built-in materialization.

### `require_resource_names_without_spaces`[​](#require_resource_names_without_spaces "Direct link to require_resource_names_without_spaces")

dbt now raises an error if it detects a space in a resource name, rather than a deprecation warning. Resource names should contain letters, numbers, and underscores only.

models/model name with spaces.sql

```sql
-- This model file should be renamed to model_name_with_underscores.sql
```

### `source_freshness_run_project_hooks`[​](#source_freshness_run_project_hooks "Direct link to source_freshness_run_project_hooks")

Project hooks ([`on-run-start` / `on-run-end`](https://docs.getdbt.com/reference/project-configs/on-run-start-on-run-end.md)) now run as part of the `dbt source freshness` command by default. If you have hooks that should not run before or after `dbt source freshness`, add a conditional check:

dbt\_project.yml

```yaml
on-run-start:
  - "{{ ... if flags.WHICH != 'freshness' }}"
```

### `require_generic_test_arguments_property`[​](#require_generic_test_arguments_property "Direct link to require_generic_test_arguments_property")

dbt supports parsing key-value arguments that are inputs to generic tests when specified under the `arguments` property. In the past, dbt didn't support a way to clearly disambiguate between properties that were inputs to generic tests and framework configurations, and only accepted arguments as top-level properties.

If you are on an older Core version where the flag is `false`, dbt will recognize the `arguments` property but raise the `ArgumentsPropertyInGenericTestDeprecation` warning.

Here's an example using the new `arguments` property:

model.yml

```yaml
models:
  - name: my_model_with_generic_test
    data_tests:
      - dbt_utils.expression_is_true:
          arguments:
            expression: "order_items_subtotal = subtotal"
```

Here's an example using the alternative `test_name` format:

model.yml

```yaml
models:
  - name: my_model_with_generic_test
    data_tests:
    - name: arbitrary_name
      test_name: dbt_utils.expression_is_true
      arguments:
         expression: "order_items_subtotal = subtotal"
      config:
        where: "1=1"
```

With this flag enabled, dbt will:

* Parse any key-value pairs under `arguments` in generic tests as inputs to the generic test macro.
* Raise a `MissingArgumentsPropertyInGenericTestDeprecation` warning if additional non-config arguments are specified outside of the `arguments` property.

## Flags reaching maturity[​](#flags-reaching-maturity "Direct link to Flags reaching maturity")

Several behavior change flags on the dbt platform Latest release track are planned to reach maturity on September 1, 2026, switching their default values from `false` to `true`. The September 1 date applies only to the dbt platform release tracks. The flags will still mature in the final release of dbt Core v1.12 as originally planned. For intro dates, refer to the [dbt Core behavior changes](https://docs.getdbt.com/reference/global-configs/behavior-changes.md#dbt-core-behavior-changes) table.

| Flag                                                                                                                    | Impact                                                                 |
| ----------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| [`skip_nodes_if_on_run_start_fails`](#skip_nodes_if_on_run_start_fails)                                                 | Can stop build                                                         |
| [`require_nested_cumulative_type_params`](#require_nested_cumulative_type_params)                                       | Can stop build (parse error)                                           |
| [`require_all_warnings_handled_by_warn_error`](#require_all_warnings_handled_by_warn_error)                             | Can stop build (when `--warn-error` is set)                            |
| [`require_batched_execution_for_custom_microbatch_strategy`](#require_batched_execution_for_custom_microbatch_strategy) | Behavior change for custom microbatch macros                           |
| [`state_modified_compare_more_unrendered_values`](#state_modified_compare_more_unrendered_values)                       | Selection-set change with potential CI impact                          |
| [`require_yaml_configuration_for_mf_time_spines`](#require_yaml_configuration_for_mf_time_spines)                       | Suppresses a deprecation warning (no functional change)                |
| [`validate_macro_args`](#validate_macro_args)                                                                           | New warning for mismatched macro arguments; errors with `--warn-error` |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### `skip_nodes_if_on_run_start_fails`[​](#skip_nodes_if_on_run_start_fails "Direct link to skip_nodes_if_on_run_start_fails")

If your project uses `on-run-start` hooks for non-critical work (for example, telemetry, notifications, audit inserts, attaching session settings), your build will stop producing output whenever a hook fails. Tables and views that previously refreshed daily will stop updating the next time the hook fails.

For more information, refer to [`on-run-start` / `on-run-end`](https://docs.getdbt.com/reference/project-configs/on-run-start-on-run-end.md).

 Recommended actions

If every model is skipped after an `on-run-start` hook fails, find which `on-run-start` hook failed in the run log — it appears as an `ERROR` immediately before all the nodes marked `SKIP`. From there, either remove the hook if it's not required, or fix the root cause of the failure by reviewing the hook's SQL statements.

To opt out of this behavior, set the flag to `false`:

dbt\_project.yml

```yaml
flags:
  skip_nodes_if_on_run_start_fails: false
```

### `require_nested_cumulative_type_params`[​](#require_nested_cumulative_type_params "Direct link to require_nested_cumulative_type_params")

Any project with a cumulative metric still using the un-nested syntax stops parsing entirely on the first command. Because parsing fails, the error affects every dbt command: `run`, `build`, `test`, `compile`, `docs generate`, the Semantic Layer, and more.

 Recommended actions

If `dbt parse` fails with `Semantic Manifest validation failed` referencing one or more cumulative metrics, migrate the metrics by moving `window` and `grain_to_date` from `type_params` into `type_params.cumulative_type_params`. Alternatively, you can migrate to the [latest YAML spec](https://docs.getdbt.com/docs/build/latest-metrics-spec.md#type_params), which removes `type_params` entirely.

```yaml
# Legacy
metrics:
  - name: weekly_active_users
    type: cumulative
    type_params:
      measure: distinct_users
      window: 7 days

# Nested
metrics:
  - name: weekly_active_users
    type: cumulative
    type_params:
      measure: distinct_users
      cumulative_type_params:
        window: 7 days

# Latest spec
metrics:
  - name: weekly_active_users
    type: cumulative
    input_metric: distinct_users
    window: 7 days
```

Re-run `dbt parse` to confirm the manifest validates.

To opt out of this behavior, set the flag to `false`:

dbt\_project.yml

```yaml
flags:
  require_nested_cumulative_type_params: false
```

### `require_all_warnings_handled_by_warn_error`[​](#require_all_warnings_handled_by_warn_error "Direct link to require_all_warnings_handled_by_warn_error")

This only affects projects that use `warn_error: true` or `--warn-error` — common in CI or in dbt platform production jobs configured for strict mode. Projects without `--warn-error` are not affected.

Example warnings that switch from "log only" to "error" for `warn_error: true` users after the flag matures:

* `JinjaLogWarning` — emitted for column types declared for non-existent seed columns
* `WarnStateTargetEqual` — emitted when the `--state` and `--target` directories are the same path
* `SelectExcludeIgnoredWithSelectorWarning` — emitted when the legacy `--selector` flag is combined with `--select` or `--exclude`
* `RunResultWarningMessage` — emitted when a test or model returns `WARN` status
* Various adapter-level warnings not previously handled by `--warn-error`

 Recommended actions

If a job using `warn_error: true` or `--warn-error` fails with `EventCompilationError` at parse or run time for an event class that previously only appeared as a log line, you can silence specific warnings with `warn_error_options`:

dbt\_project.yml

```yaml
flags:
  warn_error_options:
    silence:
      - JinjaLogWarning
      - WarnStateTargetEqual
      - RunResultWarningMessage
      - SelectExcludeIgnoredWithSelectorWarning
```

You can also use the `warn` list to keep specific events as warnings even when `--warn-error` would otherwise escalate them to errors.

To opt out of this behavior, set the flag to `false`:

dbt\_project.yml

```yaml
flags:
  require_all_warnings_handled_by_warn_error: false
```

### `require_batched_execution_for_custom_microbatch_strategy`[​](#require_batched_execution_for_custom_microbatch_strategy "Direct link to require_batched_execution_for_custom_microbatch_strategy")

This flag is only relevant if your project has a custom `get_incremental_microbatch_sql` macro. If you don't have a custom microbatch macro, you don't need to set this flag.

If you have overridden `get_incremental_microbatch_sql` — typically to work around an adapter limitation or implement a custom partition strategy — your macro is invoked under a batched contract for which it was never written. Possible outcomes:

* The macro ignores the smaller `event_time_start` / `event_time_end` window and re-processes the full range every batch, leading to wasted compute, duplicate rows, or `MERGE`/`INSERT` conflicts.
* The macro errors because it expects a single invocation per run.
* Row counts in the destination table differ between flag values.

Projects without a custom microbatch macro are unaffected; the built-in macro already runs in batches.

 Recommended actions

If a microbatch model errors mid-run, produces duplicate rows or `MERGE`/`INSERT` conflicts, or reprocesses the full date range for every batch, check whether the `get_incremental_microbatch_sql` macro filters by the `event_time_start` / `event_time_end` window. If it uses a hard-coded date filter (for example, `here event_ts >= current_date - interval '1 day'`) or reads only `_dbt_max_partition`, the macro ignores the per-batch window and reprocesses the full range on every iteration.

To fix, rewrite the macro to use the per-batch window. For example:

```sql
{% macro get_incremental_microbatch_sql(arg_dict) %}
    {%- set target = arg_dict["target_relation"] -%}
    {%- set source = arg_dict["temp_relation"] -%}
    {%- set event_time = arg_dict["incremental_predicates"] -%}

    -- Use the per-batch window dbt passes via event_time_start/event_time_end
    -- (exposed through arg_dict["incremental_predicates"] or model.config.event_time_*),
    -- NOT a hard-coded date filter.
    insert into {{ target }}
    select * from {{ source }}
{% endmacro %}
```

To verify, run `dbt run --event-time-start <date> --event-time-end <date> -s <model>` for a single batch and confirm the row count matches that single day.

To opt out of this behavior, set the flag to `false`:

dbt\_project.yml

```yaml
flags:
  require_batched_execution_for_custom_microbatch_strategy: false
```

### `state_modified_compare_more_unrendered_values`[​](#state_modified_compare_more_unrendered_values "Direct link to state_modified_compare_more_unrendered_values")

Setting the default to `true` for this flag silently changes the `state:modified` selection set that most CI, Slim CI, and `dbt build --defer` workflows rely on. There are two ways this surfaces:

* **False "modified" on the first run after the flag is set to `true`.** If the baseline manifest was captured before the flag is set (rendered values stored) and the current parse runs after the setting change (literal text stored), every node whose YAML config contains Jinja will appear as `state:modified`, even if nothing has changed. This causes a full rebuild on the first CI run after the upgrade.
* **New positives going forward.** After both manifests are captured with the flag enabled, `state:modified` will catch cases where two equivalent Jinja expressions render to the same value (for example, switching from `"{{ env_var('MAT', 'view') }}"` to `view`).

 What to expect

On the first CI or Slim CI run after the flag is set, any node whose YAML config uses Jinja (`env_var`, `var`, conditional materialization) may appear as `state:modified` even if nothing changed. This is because the baseline manifest stored rendered values while the new parse stores literal Jinja text — the two sides of the comparison differ on serialization, not on real changes.

Once your production job runs once on the **Latest** release track and generates a new baseline manifest, both sides of the `state:modified` comparison use the same format and the extra diffs disappear.

No code change is required. The cost is one extra rebuild cycle for affected nodes.

This new behavior fixes the legacy behavior. Previously, the comparison silently missed real changes to Jinja expressions (for example, switching from `"{{ env_var('MAT', 'view') }}"` to `view` renders identically and was never caught). With this flag enabled, `state:modified` is more accurate going forward.

To opt out, set the flag to `false` in `dbt_project.yml`:

dbt\_project.yml

```yaml
flags:
  state_modified_compare_more_unrendered_values: false
```

Opting out is recommended during a production deploy freeze or if your project has heavy Jinja in YAML configs and the one-time rebuild would exceed your CI compute budget. Otherwise, it is recommended to have this flag enabled.

### `require_yaml_configuration_for_mf_time_spines`[​](#require_yaml_configuration_for_mf_time_spines "Direct link to require_yaml_configuration_for_mf_time_spines")

This flag has no functional impact; the legacy time-spine model continues to work in both cases. The only visible changes are:

* The `MFTimespineWithoutYamlConfigurationDeprecation` warning no longer appears in logs.
* If you use `--warn-error`, the warning no longer fires and will no longer escalate to an error.

For more information, refer to [MetricFlow timespine](https://docs.getdbt.com/docs/build/metricflow-time-spine.md).

 Opting out of this flag

No action is required for most projects. The legacy `metricflow_time_spine.sql` model continues to work with or without this flag.

If you rely on the `MFTimespineWithoutYamlConfigurationDeprecation` warning firing under `--warn-error` to enforce a YAML migration, you can opt out by setting the flag to `false` in `dbt_project.yml`:

dbt\_project.yml

```yaml
flags:
  require_yaml_configuration_for_mf_time_spines: false
```

To remove the deprecation warning permanently, migrate `metricflow_time_spine.sql` to a YAML `time_spine` block under a model entry in `models:`. Refer to [MetricFlow timespine](https://docs.getdbt.com/docs/build/metricflow-time-spine.md) for the current syntax.

### `validate_macro_args`[​](#validate_macro_args "Direct link to validate_macro_args")

On its own, the flag emits warnings and builds continue. However, these warnings use the force-handled path and respect `--warn-error`, so projects with `--warn-error` set will see build failures at parse time.

This affects projects where the `arguments:` listed in a macro's YAML patch no longer match the macro's actual Jinja signature. For those projects, every command fails at parse time until you either update the YAML arguments to match the macro or remove the `arguments:` block entirely.

 Recommended actions

If `InvalidMacroAnnotation` warnings are appearing at parse time (or causing parse failures with `--warn-error` set), check each log line — it names the macro and the specific mismatch. In the `macros:` YAML block for each affected macro, align the `arguments:` entries with the `{% macro name(args) %}` declaration in the `.sql` file, or remove the `arguments:` block entirely.

To silence the warnings without fixing the YAML, use `warn_error_options`:

dbt\_project.yml

```yaml
flags:
  warn_error_options:
    silence:
      - InvalidMacroAnnotation
```

To opt out of this behavior, set the flag to `false`:

dbt\_project.yml

```yaml
flags:
  validate_macro_args: false
```
