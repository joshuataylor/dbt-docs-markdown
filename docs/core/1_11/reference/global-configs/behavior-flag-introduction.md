# Introduced behavior flags

Behavior change flags reaching maturity

Several behavior change flags on the dbt platform **Latest** release track are planned to be enabled by default on September 1, 2026. Refer to [Flags reaching maturity](https://docs.getdbt.com/reference/global-configs/behavior-flag-maturity.md#flags-reaching-maturity) to see which flags are affected, how they may impact your project, and how to opt out before the change takes effect.

The sections below document flags that have not yet reached maturity (default still `false`). For intro and maturity dates, refer to the [dbt Core behavior changes](https://docs.getdbt.com/reference/global-configs/behavior-changes.md#dbt-core-behavior-changes) table.

### Failures in on-run-start hooks[​](#failures-in-on-run-start-hooks "Direct link to Failures in on-run-start hooks")

This flag is planned to reach maturity on the dbt platform **Latest** release track. Before it takes effect, review [how it may impact your project](https://docs.getdbt.com/reference/global-configs/behavior-flag-maturity.md#skip_nodes_if_on_run_start_fails).

The flag is `false` by default.

Set the `skip_nodes_if_on_run_start_fails` flag to `true` to skip all selected resources from running if there is a failure on an `on-run-start` hook.

### Source definitions for state<!-- -->:modified[​](#source-definitions-for-statemodified "Direct link to source-definitions-for-statemodified")

This flag is planned to reach maturity on the dbt platform **Latest** release track. Before it takes effect, review [how it may impact your project](https://docs.getdbt.com/reference/global-configs/behavior-flag-maturity.md#state_modified_compare_more_unrendered_values).

info

You need to build the state directory using dbt v1.9 or higher, or [the dbt "Latest" release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md), and you need to set `state_modified_compare_more_unrendered_values` to `true` within your dbt\_project.yml.

If the state directory was built with an older dbt version or if the `state_modified_compare_more_unrendered_values` behavior change flag was either not set or set to `false`, you need to rebuild the state directory to avoid false positives during state comparison with `state:modified`.

The flag is `false` by default.

Set `state_modified_compare_more_unrendered_values` to `true` to reduce false positives during `state:modified` checks (especially when configs differ by target environment like `prod` vs. `dev`).

Setting the flag to `true` changes the `state:modified` comparison from using rendered values to unrendered values instead. It accomplishes this by persisting `unrendered_config` during model parsing and `unrendered_database` and `unrendered_schema` configs during source parsing.

### No spaces in source and semantic model names[​](#no-spaces-in-source-and-semantic-model-names "Direct link to No spaces in source and semantic model names")

The `require_source_and_semantic_model_names_without_spaces` flag is set to `false` by default.

Source names and semantic model names should contain letters, numbers, and underscores — *not* spaces. dbt raises the [`ResourceNamesWithSpacesDeprecation`](https://docs.getdbt.com/reference/deprecations.md#resourcenameswithspacesdeprecation) warning if it detects a space in a source name or semantic model name. When the `require_source_and_semantic_model_names_without_spaces` flag is set to `true`, dbt raises an error.

### MetricFlow time spine YAML[​](#metricflow-time-spine-yaml "Direct link to MetricFlow time spine YAML")

This flag is planned to reach maturity on the dbt platform **Latest** release track. Before it takes effect, review [how it may impact your project](https://docs.getdbt.com/reference/global-configs/behavior-flag-maturity.md#require_yaml_configuration_for_mf_time_spines).

The `require_yaml_configuration_for_mf_time_spines` flag is set to `false` by default.

In previous versions (dbt Core 1.8 and earlier), the MetricFlow time spine configuration was stored in a `metricflow_time_spine.sql` file.

When the flag is set to `true`, dbt will continue to support the SQL file configuration. When the flag is set to `false`, dbt will raise a deprecation warning if it detects a MetricFlow time spine configured in a config block in a SQL file.

The MetricFlow properties YAML file should have the `time_spine:` field. Refer to [MetricFlow timespine](https://docs.getdbt.com/docs/build/metricflow-time-spine.md) for more details.

### Custom microbatch strategy[​](#custom-microbatch-strategy "Direct link to Custom microbatch strategy")

This flag is planned to reach maturity on the dbt platform **Latest** release track. Before it takes effect, review [how it may impact your project](https://docs.getdbt.com/reference/global-configs/behavior-flag-maturity.md#require_batched_execution_for_custom_microbatch_strategy).

The `require_batched_execution_for_custom_microbatch_strategy` flag is set to `false` by default and is only relevant if you already have a custom microbatch macro in your project. If you don't have a custom microbatch macro, you don't need to set this flag as dbt will handle microbatching automatically for any model using the [microbatch strategy](https://docs.getdbt.com/docs/build/incremental-microbatch.md#how-microbatch-compares-to-other-incremental-strategies).

Set the flag to `true` if you have a custom microbatch macro set up in your project. When the flag is set to `true`, dbt will execute the custom microbatch strategy in batches.

If you have a custom microbatch macro and the flag is left as `false`, dbt will issue a deprecation warning.

Previously, users needed to set the `DBT_EXPERIMENTAL_MICROBATCH` environment variable to `true` to prevent unintended interactions with existing custom incremental strategies. But this is no longer necessary, as setting `DBT_EXPERMINENTAL_MICROBATCH` will no longer have an effect on runtime functionality.

### Cumulative metrics[​](#cumulative-metrics "Direct link to Cumulative metrics")

This flag is planned to reach maturity on the dbt platform **Latest** release track. Before it takes effect, review [how it may impact your project](https://docs.getdbt.com/reference/global-configs/behavior-flag-maturity.md#require_nested_cumulative_type_params).

[Cumulative-type metrics](https://docs.getdbt.com/docs/build/cumulative.md#parameters) are nested under the `cumulative_type_params` field in [the dbt **Latest** release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md), dbt Core v1.9 and newer. Currently, dbt will warn users if they have cumulative metrics improperly nested. To enforce the new format (resulting in an error instead of a warning), set the `require_nested_cumulative_type_params` to `true`.

Use the following metric configured with the syntax before v1.9 as an example:

```yaml
    type: cumulative
    type_params:
      measure: order_count
      window: 7 days
```

If you run `dbt parse` with that syntax on Core v1.9 or [the dbt **Latest** release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md), you will receive a warning like:

```bash
15:36:22  [WARNING]: Cumulative fields `type_params.window` and
`type_params.grain_to_date` has been moved and will soon be deprecated. Please
nest those values under `type_params.cumulative_type_params.window` and
`type_params.cumulative_type_params.grain_to_date`. See documentation on
behavior changes:
https://docs.getdbt.com/reference/global-configs/behavior-changes
```

If you set `require_nested_cumulative_type_params` to `true` and re-run `dbt parse` you will now receive an error like:

```bash
21:39:18  Cumulative fields `type_params.window` and `type_params.grain_to_date` should be nested under `type_params.cumulative_type_params.window` and `type_params.cumulative_type_params.grain_to_date`. Invalid metrics: orders_last_7_days. See documentation on behavior changes: https://docs.getdbt.com/reference/global-configs/behavior-changes.
```

Once the metric is updated, it will work as expected:

```yaml
    type: cumulative
    type_params:
      measure:
        name: order_count
      cumulative_type_params:
        window: 7 days
```

### Null-safe equality (equals macro)[​](#null-safe-equality "Direct link to Null-safe equality (equals macro)")

The `enable_truthy_nulls_equals_macro` flag is `false` by default. Setting it to `true` in your `dbt_project.yml` enables null-safe equality in the dbt [equals](https://docs.getdbt.com/reference/dbt-jinja-functions/cross-database-macros.md#equals) macro, which is used in incremental and snapshot materializations.

By default, the `equals()` macro follows SQL's [three-valued logic (3VL)](https://modern-sql.com/concept/three-valued-logic), so `NULL = NULL` evaluates to `UNKNOWN` rather than `TRUE`.

When the `enable_truthy_nulls_equals_macro` flag is enabled, the `equals()` macro uses the semantics of the [`IS NOT DISTINCT FROM`](https://modern-sql.com/feature/is-distinct-from) operator with two `NULL` values treated as equal.

To enable the flag, add it under `flags` in `dbt_project.yml`:

dbt\_project.yml

```yml
flags:
  enable_truthy_nulls_equals_macro: true
```

### Macro argument validation[​](#macro-argument-validation "Direct link to Macro argument validation")

This flag is planned to reach maturity on the dbt platform **Latest** release track. Before it takes effect, review [how it may impact your project](https://docs.getdbt.com/reference/global-configs/behavior-flag-maturity.md#validate_macro_args).

dbt supports optional validation for macro arguments using the `validate_macro_args` flag. By default, the `validate_macro_args` flag is set to `false`, which means that dbt won't validate the names or types of documented macro arguments.

In the past, dbt didn't enforce a standard vocabulary for the [`type`](https://docs.getdbt.com/reference/resource-properties/arguments.md#type) field on macro arguments in YAML. Because of this, the `type` field was used for documentation only, and dbt didn't check that:

* the argument names matched those in your macro
* the argument types were valid or consistent with the macro's Jinja definition

Here's an example of a documented macro:

macros/filename.yml

```yaml
macros:
  - name: <macro name>
    arguments:
      - name: <arg name>
        type: <string>
```

When you set the `validate_macro_args` flag to `true`, dbt will:

* Validate macro arguments during project parsing.
* Check that all argument names in your YAML match those in the macro definition.
* Raise warnings if the names or types don't match.
* Validate that the [`type` values follow the supported format](https://docs.getdbt.com/reference/resource-properties/arguments.md#supported-types).
* If no arguments are documented in the YAML, infer them from the macro and include them in the [`manifest.json` file](https://docs.getdbt.com/reference/artifacts/manifest-json.md).

 When does validation occur?

Macro argument validation runs during project parsing, not during macro execution. Any dbt command that parses the project will trigger validation if you enable the `validate_macro_args` flag.

* In dbt Core:

  <!-- -->

  * Validation runs as part of parsing for most commands (`parse`, `build`, `run`, `test`, `seed`, `snapshot`, `compile`).
  * With a full parse, dbt validates all macros.
  * With partial parsing (the default), dbt validates only macros affected by changed files.
  * Use `--no-partial-parse` to force validation of all macros.

### Warn-error handler for all warnings[​](#warn-error-handler-for-all-warnings "Direct link to Warn-error handler for all warnings")

This flag is planned to reach maturity on the dbt platform **Latest** release track. Before it takes effect, review [how it may impact your project](https://docs.getdbt.com/reference/global-configs/behavior-flag-maturity.md#require_all_warnings_handled_by_warn_error).

By default, the `require_all_warnings_handled_by_warn_error` flag is set to `false`.

When you set `require_all_warnings_handled_by_warn_error` to `true`, all warnings raised during a run are routed through the `--warn-error` / `--warn-error-options` handler. This ensures consistent behavior when promoting warnings to errors or silencing them. When the flag is `false`, only some warnings are processed by the handler while others may bypass it.

Note that enabling this for projects that use `--warn-error` (or `--warn-error-options='{"error":"all"}'`) may cause builds to fail on warnings that were previously ignored. We recommend enabling it gradually.

 Recommended steps to enable the flag

We recommend the following rollout plan when setting the `require_all_warnings_handled_by_warn_error` flag to `true`:

1. Run a full build without partial parsing to surface parse-time warnings, and confirm it finishes successfully:

   ```bash
   dbt build --no-partial-parse
   ```

   * Some warnings are only emitted at parse time.
   * If the build fails because warnings are already treated as errors (via `--warn-error` or `--warn-error-options`), fix those first and re-run.

2. Review the logs:

   * If you have any warnings at this point, it means they weren't handled by `--warn-error`/`--warn-error-options`. Continue to the next step.
   * If there are no warnings, enable the flag in all environments and that's it!

3. Enable `require_all_warnings_handled_by_warn_error` in your development environment and fix any warnings that now surface as errors.

4. Enable the flag in your CI environment (if you have one) and ensure builds pass.

5. Enable the flag in your production environment.

### Unique project resource names[​](#unique-project-resource-names "Direct link to Unique project resource names")

The `require_unique_project_resource_names` flag enforces uniqueness of resource names within the same package. dbt resources such as models, seeds, snapshots, analyses, tests, and functions share a common namespace. When two resources in the same package have the same name, dbt must decide which one a `ref()` or `source()` refers to. Previously, this check was not always enforced, which meant duplicate names could result in dbt referencing the wrong resource.

The `require_unique_project_resource_names` flag is set to `false` by default. With this setting, if two unversioned resources in the same package share the same name, dbt continues to run and raises a [`DuplicateNameDistinctNodeTypesDeprecation`](https://docs.getdbt.com/reference/deprecations.md#duplicatenamedistinctnodetypesdeprecation) warning. When set to `true`, dbt raises a `DuplicateResourceNameError` error.

For example, if your project contains a model and a seed named `sales`:

```text
models/sales.sql
seeds/sales.csv
```

And a model contains:

```sql
select * from {{ ref('sales') }}
```

When the flag is set to `true`, dbt will raise:

```text
DuplicateResourceNameError: Found resources with the same name 'sales' in package 'project': 'model.project.sales' and 'seed.project.sales'. Please update one of the resources to have a unique name.
```

When this error is raised, you should rename one of the resources, or refactor the project structure to avoid name conflicts.

### Package `ref` search order[​](#package-ref-search-order "Direct link to package-ref-search-order")

The `require_ref_searches_node_package_before_root` flag controls the search order when dbt resolves `ref()` calls defined within a package.

The flag is set to `false` by default in **Latest** and dbt Core v1.11. When dbt resolves a `ref()` in a package model, it searches for the referenced model in the root project *first*, then in the package where the model is defined.

For example, the following model in the package `my_package` is imported by the project `my_project`:

my\_package/model\_downstream.sql

```sql
select * from {{ ref('model_upstream') }}
```

By default, dbt searches for `model_upstream` in this order:

1. First in `my_project` (root project)
2. Then in `my_package` (where the model is defined)

When you set the `require_ref_searches_node_package_before_root` flag to `true`, dbt searches the package where the model is defined *before* searching the root project.

Using the same example, dbt searches for `model_upstream` in this order:

1. First in `my_package` (where the model is defined)
2. Then in `my_project` (root project)

The current default behavior is considered a [bug in dbt-core](https://github.com/dbt-labs/dbt-core/issues/11351) because it can *potentially* lead to unexpected dependency cycles. However, because this is long-standing behavior, changing the default requires setting `require_ref_searches_node_package_before_root` to `true` to avoid breaking existing projects.

### Valid schema from `generate_schema_name`[​](#valid-schema-from-generate_schema_name "Direct link to valid-schema-from-generate_schema_name")

The `generate_schema_name` macro determines the schema where dbt creates models and other resources. Returning a `null` value from this macro can result in invalid schema names and lead to unpredictable behavior during dbt runs.

The `require_valid_schema_from_generate_schema_name` behavior flag is set to `false` by default. When `false`, dbt raises the [`GenerateSchemaNameNullValueDeprecation`](https://docs.getdbt.com/reference/deprecations.md#generateschemanamenullvaluedeprecation) warning when a custom `generate_schema_name` macro returns a `null` value.

When `require_valid_schema_from_generate_schema_name` is set to `true`, dbt enforces stricter validation and raises a parsing error.

For example, if your project has a custom `generate_schema_name` macro that returns `null`:

macros/get\_custom\_schema.sql

```sql
{% macro generate_schema_name(custom_schema_name, node) -%}
    {%- if custom_schema_name is none -%}
        {{ return(none) }}
    {%- else -%}
        {{ custom_schema_name | trim }}
    {%- endif -%}
{%- endmacro %}
```

With the default behavior, dbt raises a deprecation warning. When `require_valid_schema_from_generate_schema_name` is set to `true`, dbt raises an error.

To resolve this, update your macro to return a valid schema name (`target.schema` in this example):

macros/get\_custom\_schema.sql

```sql
{% macro generate_schema_name(custom_schema_name, node) -%}
    {%- if custom_schema_name is none -%}
        {{ return(target.schema) }}
    {%- else -%}
        {{ custom_schema_name | trim }}
    {%- endif -%}
{%- endmacro %}
```

### `sql_header` in data tests[​](#sql_header-in-data-tests "Direct link to sql_header-in-data-tests")

Set the `require_sql_header_in_test_configs` flag to `true` to enable support for the [`sql_header`](https://docs.getdbt.com/reference/resource-configs/sql_header.md) config for generic data tests. When enabled, you can set `sql_header` in the `config` of a generic data test at the model or column level in your `properties.yml` file. You can use `sql_header` to define SQL that should run before the test executes (for example, to create temporary functions, to set session parameters, or to declare variables required by the test query). dbt runs this SQL before executing the test.

For example:

models/properties.yml

```yaml
models:
  - name: orders
    columns:
      - name: order_id
        data_tests:
          - not_null:
              name: not_null_orders_order_id
              config:
                sql_header: "-- SQL_HEADER_TEST_MARKER"
```

For more information, refer to [Data test configurations](https://docs.getdbt.com/reference/data-test-configs.md).

### Project-level configuration for analyses [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[​](#project-level-configuration-for-analyses "Direct link to project-level-configuration-for-analyses")

Beta feature

The project-level configuration for analyses is a beta feature in dbt Core v1.12.

Previously, project-level configuration for [analyses](https://docs.getdbt.com/docs/build/analyses.md) in `dbt_project.yml` was silently ignored. Fully qualified names (FQNs) for analyses also contained an extra `analyses` path segment that was inconsistent with other resource types.

When `require_corrected_analysis_fqns` is set to `true`, dbt:

* Routes analysis configurations from the `analyses` block in `dbt_project.yml`, enabling project-level configurations to take effect.
* Removes the extra FQN segment so that analysis FQNs are consistent with other resource types (for example, `your_project.my_analysis` instead of `your_project.analyses.my_analysis`).

To configure analyses at the project level, set the [`require_corrected_analysis_fqns`](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md#project-level-configuration-for-analyses) flag to `true` and add an `analyses` block in your `dbt_project.yml`. The project-level configuration applies to existing analyses in the `analyses/` folder — for example, setting `+enabled: false` disables them all.

dbt\_project.yml

```yaml
flags:
  require_corrected_analysis_fqns: true

analyses:
  +enabled: true | false
```

For more information, refer to [Analyses](https://docs.getdbt.com/docs/build/analyses.md) and [Analysis properties](https://docs.getdbt.com/reference/analysis-properties.md).

### Jinja file extensions [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[​](#jinja-file-extensions "Direct link to jinja-file-extensions")

Beta feature

Support for Jinja file extensions is a beta feature in dbt Core v1.12.

The `allow_jinja_file_extensions` flag is set to `false` by default.

When set to `true`, dbt recognizes Jinja-style extension suffixes (for example,`.j2`, `.jinja`, and `.jinja2`) appended to `.sql` and `.md` files. This lets you use Jinja-aware syntax highlighting in IDEs that associate these suffixes with Jinja templating.

dbt strips the Jinja suffix when determining node names; resource names remain unchanged regardless of whether the Jinja suffix is present. For example, a [docs block](https://docs.getdbt.com/docs/build/documentation.md#using-docs-blocks) file named `my_docs.md.j2` is parsed identically to `my_docs.md`, and a model file named `my_model.sql.j2` is parsed as the model `my_model`.

When this flag is `false` or unset, dbt ignores files with these suffixes without logging a warning. If you've already added schema properties for that file, you'll see a "Did not find matching node for patch warning on schema.yml" warning.

### Latest version pointer for versioned models [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[​](#latest-version-pointer-for-versioned-models "Direct link to latest-version-pointer-for-versioned-models")

Beta feature

The `latest_version_pointer_enabled_by_default` flag is a beta feature in dbt Core v1.12.

The `latest_version_pointer_enabled_by_default` flag is set to `false` by default.

When you set it to `true`, dbt automatically creates a [latest version pointer](https://docs.getdbt.com/docs/mesh/govern/model-versions.md#pointing-to-the-latest-version) view for every versioned model in the project, without requiring per-model configuration. The pointer view is named after the model's base name (for example, `dim_customers`) and always points to the relation for the model with `is_latest_version: true` (for example, `dim_customers_v2`).

Without this flag, you must opt in per model by setting [`latest_version_pointer.enabled: true`](https://docs.getdbt.com/reference/resource-configs/latest_version_pointer.md) in the model config.
