# Upgrading to the dbt Fusion engine (v2.0)

important

The dbt Fusion engine is currently available for installation in:

* [Local command line interface (CLI) tools](https://docs.getdbt.com/docs/fusion/install-fusion-cli.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [VS Code and Cursor with the dbt extension](https://docs.getdbt.com/docs/install-dbt-extension.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [dbt platform environments](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-version-in-cloud.md#dbt-fusion-engine) [Private preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Join the conversation in our Community Slack channel [`#dbt-fusion-engine`](https://getdbt.slack.com/archives/C088YCAB6GH).

Read the [Fusion Diaries](https://github.com/dbt-labs/dbt-fusion/discussions/categories/announcements) for the latest updates.

<!-- -->

## More information about Fusion[​](#more-information-about-fusion "Direct link to More information about Fusion")

Fusion marks a significant update to dbt. While many of the workflows you've grown accustomed to remain unchanged, there are a lot of new ideas, and a lot of old ones going away. The following is a list of the full scope of our current release of the Fusion engine, including implementation, installation, deprecations, and limitations:

* [About the dbt Fusion engine](https://docs.getdbt.com/docs/fusion/about-fusion.md)
* [About the dbt extension](https://docs.getdbt.com/docs/about-dbt-extension.md)
* [New concepts in Fusion](https://docs.getdbt.com/docs/fusion/new-concepts.md)
* [Supported features matrix](https://docs.getdbt.com/docs/fusion/supported-features.md)
* [Installing Fusion CLI](https://docs.getdbt.com/docs/fusion/install-fusion-cli.md)
* [Installing VS Code extension](https://docs.getdbt.com/docs/install-dbt-extension.md)
* [Fusion release track](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-version-in-cloud.md#dbt-fusion-engine)
* [Quickstart for Fusion](https://docs.getdbt.com/guides/fusion.md?step=1)
* [Upgrade guide](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-fusion.md)
* [Fusion licensing](http://www.getdbt.com/licenses-faq)

## What to know before upgrading[​](#what-to-know-before-upgrading "Direct link to What to know before upgrading")

dbt Core and dbt Fusion share a common language spec—the code in your project. dbt Labs is committed to providing feature parity with dbt Core wherever possible.

At the same time, we want to take this opportunity to *strengthen the framework* by removing deprecated functionality, rationalizing confusing behavior, and providing more rigorous validation on erroneous inputs. This means that there is some work involved in preparing an existing dbt project for readiness on Fusion.

That work is documented below — it should be simple, straightforward, and in many cases, auto-fixable with the [`dbt-autofix`](https://github.com/dbt-labs/dbt-autofix) helper.

You can find more information about what's changing in the dbt Fusion engine [changelog](https://github.com/dbt-labs/dbt-fusion/blob/main/CHANGELOG.md).

#### Upgrade considerations[​](#upgrade-considerations "Direct link to Upgrade considerations")

Keep in mind the following considerations during the upgrade process:

* **Manifest incompatibility** — Fusion is backwards-compatible and can read dbt Core [manifests](https://docs.getdbt.com/reference/artifacts/manifest-json.md). However, dbt Core isn't forward-compatible and can't read Fusion manifests. Fusion produces a `v20` manifest, while the latest version of dbt Core still produces a `v12` manifest.

  As a result, mixing dbt Core and Fusion manifests across environments breaks cross-environment features. To avoid this, use `state:modified`, `--defer`, and cross-environment `dbt docs generate` only after *all* environments are running the latest Fusion version. Using these features before all environments are on Fusion may cause errors and failures.

* **State-aware orchestration** — If using [state-aware orchestration](https://docs.getdbt.com/docs/deploy/state-aware-about.md), dbt doesn’t detect a change if a table or view is dropped outside of dbt, as the cache is unique to each dbt platform environment. This means state-aware orchestration will not rebuild that model until either there is new data or a change in the code that the model uses.

  * **Workarounds:**

    * Use the **Clear cache** button on the target Environment page to force a full rebuild (acts like a reset), or
    * Temporarily disable State-aware orchestration for the job and rerun it.

### Supported adapters[​](#supported-adapters "Direct link to Supported adapters")

The following adapters are supported in the dbt Fusion engine:

 BigQuery

* Service Account / User Token
* Native OAuth
* External OAuth
* [Required permissions](https://docs.getdbt.com/docs/core/connect-data-platform/bigquery-setup.md#required-permissions)

 Databricks

* Service Account / User Token
* Native OAuth

 Redshift

* Username / Password
* IAM profile

 Snowflake

* Username / Password
* Native OAuth
* External OAuth
* Key pair using a modern PKCS#8 method
* MFA

### A clean slate[​](#a-clean-slate "Direct link to A clean slate")

dbt Labs is committed to moving forward with Fusion, and it will not support any deprecated functionality (see the [Changes overview](https://docs.getdbt.com/reference/changes-overview.md) for details):

* All [deprecation warnings](https://docs.getdbt.com/reference/deprecations.md) must be resolved before upgrading to the new engine. This includes historic deprecations and [new ones as of dbt Core v1.10](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v1.10.md#deprecation-warnings).
* All [behavior change flags](https://docs.getdbt.com/reference/global-configs/behavior-changes.md#behaviors) will be removed (generally enabled). You can no longer opt out of them using `flags:` in your `dbt_project.yml`.

### Ecosystem packages[​](#ecosystem-packages "Direct link to Ecosystem packages")

The most popular `dbt-labs` packages (`dbt_utils`, `audit_helper`, `dbt_external_tables`, `dbt_project_evaluator`) are already compatible with Fusion. External packages published by organizations outside of dbt may use outdated code or incompatible features that fail to parse with the new Fusion engine. We're working with those package maintainers to make packages available for Fusion. Packages requiring an upgrade to a new release for Fusion compatibility, will be documented in this upgrade guide.

### Changed functionality[​](#changed-functionality "Direct link to Changed functionality")

When developing the Fusion engine, there were opportunities to improve the dbt framework - failing earlier (when possible), fixing bugs, optimizing run order, and deprecating flags that are no longer relevant. The result is a handful of specific and nuanced changes to existing behavior.

When upgrading to Fusion, you should expect the following changes in functionality:

#### Parse time printing of relations will print out the full qualified name, instead of an empty string[​](#parse-time-printing-of-relations-will-print-out-the-full-qualified-name-instead-of-an-empty-string "Direct link to Parse time printing of relations will print out the full qualified name, instead of an empty string")

In dbt Core v1, when printing the result of `get_relation()`, the parse time output for that Jinja would print `None` (the undefined object coerces to the string “None”).

In Fusion, to help with intelligent batching of `get_relation()` calls (and significantly speed up `dbt compile`), dbt needs to construct a relation object with the fully qualified name resolved at parse time for the `get_relation()` adapter call.

Constructing a relation object with the fully qualified name in Fusion produces different behavior than dbt Core v1 in `print()`, `log()`, or any Jinja macro that outputs to `stdout` or `stderr` at parse time.

Example:

```jinja
{% set relation = adapter.get_relation(
database=db_name,
schema=db_schema,
identifier='a')
%}
{{ print('relation: ' ~ relation) }}

{% set relation_via_api = api.Relation.create(
database=db_name,
schema=db_schema,
identifier='a'
) %}
{{ print('relation_via_api: ' ~ relation_via_api) }}
```

The output after `dbt parse` in dbt Core v1:

```text
relation: None
relation_via_api: my_db.my_schema.my_table
```

The output after `dbt parse` in Fusion:

```text
relation: my_db.my_schema.my_table
relation_via_api: my_db.my_schema.my_table
```

#### Deprecated flags[​](#deprecated-flags "Direct link to Deprecated flags")

What are "deprecated flags"?

Deprecated flags are command-line flags (like `--models`, `--print`) that you pass to dbt commands. These are being removed in Fusion.

This is different from:

* [Deprecation warnings](https://docs.getdbt.com/reference/deprecations.md) — Features in your project code (models, YAML, macros) that need to be updated
* [Behavior change flags](https://docs.getdbt.com/reference/global-configs/behavior-changes.md) — Flags in `dbt_project.yml` that let you opt in/out of new behaviors

See the [Changes overview](https://docs.getdbt.com/reference/changes-overview.md) for a full comparison.

Some historic CLI flags in dbt Core v1 will no longer do anything in Fusion. If you pass them into a dbt command using Fusion, the command will not error, but the flag will do nothing (and warn accordingly).

One exception to this rule: The `--models` / `--model` / `-m` flag was renamed to `--select` / `--s` way back in dbt Core v0.21 (Oct 2021). Silently skipping this flag means ignoring your command's selection criteria, which could mean building your entire DAG when you only meant to select a small subset. For this reason, the `--models` / `--model` / `-m` flag **will raise an error** in Fusion. Please update your job definitions accordingly.

| flag name                                                                                                                                        | remediation                                               |
| ------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------- |
| `dbt seed` [`--show`](https://docs.getdbt.com/reference/commands/seed.md)                                                                        | N/A                                                       |
| [`--print` / `--no-print`](https://docs.getdbt.com/reference/global-configs/print-output.md)                                                     | No action required                                        |
| [`--printer-width`](https://docs.getdbt.com/reference/global-configs/print-output.md#printer-width)                                              | No action required                                        |
| [`--source`](https://docs.getdbt.com/reference/commands/deps.md#non-hub-packages)                                                                | No action required                                        |
| [`--record-timing-info` / `-r`](https://docs.getdbt.com/reference/global-configs/record-timing-info.md)                                          | No action required                                        |
| [`--cache-selected-only` / `--no-cache-selected-only`](https://docs.getdbt.com/reference/global-configs/cache.md)                                | No action required                                        |
| [`--clean-project-files-only` / `--no-clean-project-files-only`](https://docs.getdbt.com/reference/commands/clean.md#--clean-project-files-only) | No action required                                        |
| `--single-threaded` / `--no-single-threaded`                                                                                                     | No action required                                        |
| `dbt source freshness` [`--output` / `-o`](https://docs.getdbt.com/docs/deploy/source-freshness.md)                                              |                                                           |
| [`--config-dir`](https://docs.getdbt.com/reference/commands/debug.md)                                                                            | No action required                                        |
| [`--resource-type` / `--exclude-resource-type`](https://docs.getdbt.com/reference/global-configs/resource-type.md)                               | change to `--resource-types` / `--exclude-resource-types` |
| `--show-resource-report` / `--no-show-resource-report`                                                                                           | No action required                                        |
| [`--log-cache-events` / `--no-log-cache-events`](https://docs.getdbt.com/reference/global-configs/logs.md#logging-relational-cache-events)       | No action required                                        |
| `--use-experimental-parser` / `--no-use-experimental-parser`                                                                                     | No action required                                        |
| [`--empty-catalog`](https://docs.getdbt.com/reference/commands/cmd-docs.md#dbt-docs-generate)                                                    |                                                           |
| [`--compile` / `--no-compile`](https://docs.getdbt.com/reference/commands/cmd-docs.md#dbt-docs-generate)                                         |                                                           |
| `--inline-direct`                                                                                                                                | No action required                                        |
| `--partial-parse-file-diff` / `--no-partial-parse-file-diff`                                                                                     | No action required                                        |
| `--partial-parse-file-path`                                                                                                                      | No action required                                        |
| `--populate-cache` / `--no-populate-cache`                                                                                                       | No action required                                        |
| `--static-parser` / `--no-static-parser`                                                                                                         | No action required                                        |
| `--use-fast-test-edges` / `--no-use-fast-test-edges`                                                                                             | No action required                                        |
| [`--introspect` / `--no-introspect`](https://docs.getdbt.com/reference/commands/compile.md#introspective-queries)                                | No action required                                        |
| `--inject-ephemeral-ctes` / `--no-inject-ephemeral-ctes`                                                                                         |                                                           |
| [`--partial-parse` / `--no-partial-parse`](https://docs.getdbt.com/reference/parsing.md#partial-parsing)                                         | No action required                                        |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

#### Conflicting package versions when a local package depends on a hub package which the root package also wants will error[​](#conflicting-package-versions-when-a-local-package-depends-on-a-hub-package-which-the-root-package-also-wants-will-error "Direct link to Conflicting package versions when a local package depends on a hub package which the root package also wants will error")

If a local package depends on a hub package that the root package also wants, `dbt deps` doesn't resolve conflicting versions in dbt Core v1; it will install whatever the root project requests.

Fusion will present an error:

```bash
error: dbt8999: Cannot combine non-exact versions: =0.8.3 and =1.1.1
```

#### Parse will fail on nonexistent macro invocations and adapter methods[​](#parse-will-fail-on-nonexistent-macro-invocations-and-adapter-methods "Direct link to Parse will fail on nonexistent macro invocations and adapter methods")

When you call a nonexistent macro in dbt:

```sql
select
  id as payment_id,
  # my_nonexistent_macro is a macro that DOES NOT EXIST
  {{ my_nonexistent_macro('amount') }} as amount_usd,
from app_data.payments
```

Or a nonexistent adapter method:

```sql
{{ adapter.does_not_exist() }}
```

In dbt Core v1, `dbt parse` passes, but `dbt compile` fails.

Fusion will error out during `parse`.

#### Parse will fail on missing generic test[​](#parse-will-fail-on-missing-generic-test "Direct link to Parse will fail on missing generic test")

When you have an undefined generic test in your project:

```yaml

models:
  - name: dim_wizards
    data_tests:
      - does_not_exist
```

In dbt Core v1, `dbt parse` passes, but `dbt compile` fails.

Fusion will error out during `parse`.

#### Parse will fail on missing variable[​](#parse-will-fail-on-missing-variable "Direct link to Parse will fail on missing variable")

When you have an undefined variable in your project:

```sql

select {{ var('does_not_exist') }} as my_column
```

In dbt Core v1, `dbt parse` passes, but `dbt compile` fails.

Fusion will error out during `parse`.

#### Stricter evaluation of duplicate docs blocks[​](#stricter-evaluation-of-duplicate-docs-blocks "Direct link to Stricter evaluation of duplicate docs blocks")

In older versions of dbt Core, it was possible to create scenarios with duplicate [docs blocks](https://docs.getdbt.com/docs/build/documentation.md#using-docs-blocks). For example, you can have two packages with identical docs blocks referenced by an unqualified name in your dbt project. In this case, dbt Core would use whichever docs block is referenced without any warnings or errors.

Fusion adds stricter evaluation of names of docs blocks to prevent such ambiguity. It will present an error if it detects duplicate names:

```bash
dbt found two docs with the same name: 'docs_block_title in files: 'models/crm/_crm.md' and 'docs/crm/business_class_marketing.md'
```

To resolve this error, rename any duplicate docs blocks.

#### End of support for legacy manifest versions[​](#end-of-support-for-legacy-manifest-versions "Direct link to End of support for legacy manifest versions")

You can no longer interoperate with pre-1.8 versions of dbt-core if you're a:

* Hybrid customer running Fusion and an old (pre-v1.8) version of dbt Core
* Customer upgrading from the old (pre-v1.8) version of dbt Core to Fusion

Fusion can not interoperate with the old manifest, which powers features like deferral for `state:modified` comparison.

#### `dbt clean` will not delete any files in configured resource paths or files outside the project directory[​](#dbt-clean-will-not-delete-any-files-in-configured-resource-paths-or-files-outside-the-project-directory "Direct link to dbt-clean-will-not-delete-any-files-in-configured-resource-paths-or-files-outside-the-project-directory")

In dbt Core v1, `dbt clean` deletes:

* Any files outside the project directory if `clean-targets` is configured with an absolute path or relative path containing `../`, though there is an opt-in config to disable this (`--clean-project-files-only` / `--no-clean-project-files-only`).
* Any files in the `asset-paths` or `doc-paths` (even though other resource paths, like `model-paths` and `seed-paths`, are restricted).

In Fusion, `dbt clean` will not delete any files in configured resource paths or files outside the project directory.

#### All unit tests are run first in `dbt build`[​](#all-unit-tests-are-run-first-in-dbt-build "Direct link to all-unit-tests-are-run-first-in-dbt-build")

In dbt Core v1, the direct parents of the model being unit tested needed to exist in the warehouse to retrieve the needed column name and type information. `dbt build` runs the unit tests (and their dependent models) *in lineage order*.

In Fusion, `dbt build` runs *all* of the unit tests *first*, and then build the rest of the DAG, due to built-in column name and type awareness.

#### Configuring `--threads`[​](#configuring---threads "Direct link to configuring---threads")

dbt Core runs with `--threads 1` by default. You can increase this number to run more nodes in parallel on the remote data platform, up to the max parallelism enabled by the DAG.

Fusion handles threading differently depending on your data platform:

| Adapter        | Behavior                                                                                                                                                                                              |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Snowflake**  | Fusion ignores user-set threads and automatically optimizes parallelism for maximum performance.<br />The only supported override is `threads: 1`, which can also help resolve timeout issues if set. |
| **Databricks** | Fusion ignores user-set threads and automatically optimizes parallelism for maximum performance.<br />The only supported override is `threads: 1`, which can also help resolve timeout issues if set. |
| **BigQuery**   | Fusion respects user-set threads to manage API rate limits.<br />Setting `--threads 0` (or omitting the setting) allows Fusion to dynamically optimize parallelism.                                   |
| **Redshift**   | Fusion respects user-set threads to manage concurrency limits.<br />Setting `--threads 0` (or omitting the setting) allows Fusion to dynamically optimize parallelism.                                |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

For more information, refer to [Using threads](https://docs.getdbt.com/docs/running-a-dbt-project/using-threads.md#fusion-engine-thread-optimization).

#### Continue to compile unrelated nodes after hitting a compile error[​](#continue-to-compile-unrelated-nodes-after-hitting-a-compile-error "Direct link to Continue to compile unrelated nodes after hitting a compile error")

As soon as dbt Core's `compile` encounters an error compiling one of your models, dbt stops and doesn't compile anything else.

When Fusion's `compile` encounters an error, it will skip nodes downstream of the one that failed to compile, but it will keep compiling the rest of the DAG (in parallel, up to the number of configured / optimal threads).

#### Seeds with extra commas don't result in extra columns[​](#seeds-with-extra-commas-dont-result-in-extra-columns "Direct link to Seeds with extra commas don't result in extra columns")

In dbt Core v1, if you have an additional comma on your seed, dbt creates a seed with an additional empty column.

For example, the following seed file (with an extra comma):

```text
animal,  
dog,  
cat,  
bear,  
```

Will produce this table when `dbt seed` is executed:

| animal | b |
| ------ | - |
| dog    |   |
| cat    |   |
| bear   |   |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Fusion will not produce this extra column in the table resulting from `dbt seed`:

| animal |
| ------ |
| dog    |
| cat    |
| bear   |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

#### Move standalone anchors under `anchors:` key[​](#move-standalone-anchors-under-anchors-key "Direct link to move-standalone-anchors-under-anchors-key")

As part of the ongoing process of making the dbt authoring language more precise, unexpected top-level keys in a YAML file will result in errors. A common use case behind these unexpected keys is standalone anchor definitions at the top level of a YAML file. You can use the new top-level `anchors:` key as a container for these reusable configuration blocks.

For example, rather than using this configuration:

models/\_models.yml

```yml
# id_column is not a valid name for a top-level key in the dbt authoring spec, and will raise an error
id_column: &id_column_alias
  name: id
  description: This is a unique identifier.
  data_type: int
  data_tests:
    - not_null
    - unique

models:
  - name: my_first_model
    columns: 
      - *id_column_alias
      - name: unrelated_column_a
        description: This column is not repeated in other models.
  - name: my_second_model
    columns: 
      - *id_column_alias
```

Move the anchor under the `anchors:` key instead:

models/\_models.yml

```yml
anchors: 
  - &id_column_alias
      name: id
      description: This is a unique identifier.
      data_type: int
      data_tests:
        - not_null
        - unique

models:
  - name: my_first_model
    columns: 
      - *id_column_alias
      - name: unrelated_column_a
        description: This column is not repeated in other models
  - name: my_second_model
    columns: 
      - *id_column_alias
```

This move is only necessary for fragments defined outside of the main YAML structure. For more information about this new key, see [anchors](https://docs.getdbt.com/reference/resource-properties/anchors.md).

#### Algebraic operations in Jinja macros[​](#algebraic-operations-in-jinja-macros "Direct link to Algebraic operations in Jinja macros")

In dbt Core, you can set algebraic functions in the return function of a Jinja macro:

```jinja
{% macro my_macro() %}

return('xyz') + 'abc'

{% endmacro %}
```

This is no longer supported in Fusion and will return an error:

```bash
error: dbt1501: Failed to add template invalid operation: return() is called in a non-block context
```

This is not a common use case and there is no deprecation warning for this behavior in dbt Core. The supported format is:

```jinja
{% macro my_macro() %}

return('xyzabc')

{% endmacro %}
```

### Accessing custom configurations in meta[​](#accessing-custom-configurations-in-meta "Direct link to Accessing custom configurations in meta")

`config.get()` and `config.require()` don't return values from the `meta` dictionary. If you try to access a key that only exists in `meta`, dbt emits a warning:

```bash
warning: The key 'my_key' was not found using config.get('my_key'), but was 
detected as a custom config under 'meta'. Please use config.meta_get('my_key') 
or config.meta_require('my_key') instead.
```

Behavior when a key exists only in meta:

| Method                     | Behavior                                       |
| -------------------------- | ---------------------------------------------- |
| `config.get('my_key')`     | Returns the default value and emits a warning. |
| `config.require('my_key')` | Raises an error and emits a warning.           |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

To access custom configurations stored under meta, use the explicit methods:

```jinja
{% set owner = config.meta_get('owner') %}
{% set has_pii = config.meta_require('pii') %}
```

For more information, see [config.meta\_get()](https://docs.getdbt.com/reference/dbt-jinja-functions/config.md#configmeta_get) and [config.meta\_require()](https://docs.getdbt.com/reference/dbt-jinja-functions/config.md#configmeta_require).

### Fusion compiler[​](#fusion-compiler "Direct link to Fusion compiler")

#### Snowflake model functions[​](#snowflake-model-functions "Direct link to Snowflake model functions")

Fusion supports [Snowflake ML model functions](https://docs.snowflake.com/en/guides-overview-ml-functions), which allow you to call machine learning models directly in SQL.

Because model function return types are flexible and defined by the underlying model, Fusion uses simplified type checking:

* **Arguments:** Fusion accepts any arguments without strict type validation.
* **Return type:** Fusion treats all model function results as `VARIANT`.

To use the result in your models, cast it to the expected type:

```sql
select 
  my_model!predict(input_column)::float as prediction_score
from {{ ref('my_table') }}
```

### Package support[​](#package-support "Direct link to Package support")

<!-- -->

To determine if a package is compatible with the dbt Fusion engine, visit the [dbt package hub](https://hub.getdbt.com/) and look for the Fusion-compatible badge, or review the package's [`require-dbt-version` configuration](https://docs.getdbt.com/reference/project-configs/require-dbt-version.md#pin-to-a-range).

* Packages with a `require-dbt-version` that equals or contains `2.0.0` are compatible with Fusion. For example, `require-dbt-version: ">=1.10.0,<3.0.0"`.

  Even if a package doesn't reflect compatibility in the package hub, it may still work with Fusion. Work with package maintainers to track updates, and [thoroughly test packages](https://docs.getdbt.com/guides/fusion-package-compat?step=5) that aren't clearly compatible before deploying.

* Package maintainers who would like to make their package compatible with Fusion can refer to the [Fusion package upgrade guide](https://docs.getdbt.com/guides/fusion-package-compat.md) for instructions.

Fivetran package considerations:

* The Fivetran `source` and `transformation` packages have been combined into a single package.
* If you manually installed source packages like `fivetran/github_source`, you need to ensure `fivetran/github` is installed and deactivate the transformation models.

<!-- -->

#### Package compatibility messages[​](#package-compatibility-messages "Direct link to Package compatibility messages")

Inconsistent Fusion warnings and `dbt-autofix` logs

Fusion warnings and `dbt-autofix` logs may show different messages about package compatibility.

If you use [`dbt-autofix`](https://github.com/dbt-labs/dbt-autofix) while upgrading to Fusion in the Studio IDE or dbt VS Code extension, you may see different messages about package compatibility between `dbt-autofix` and Fusion warnings.

Here's why:

* Fusion warnings are emitted based on a package's `require-dbt-version` and whether `require-dbt-version` contains `2.0.0`.
* Some packages are already Fusion-compatible even though package maintainers haven't yet updated `require-dbt-version`.
* `dbt-autofix` knows about these compatible packages and will not try to upgrade a package that it knows is already compatible.

This means that even if you see a Fusion warning for a package that `dbt-autofix` identifies as compatible, you don't need to change the package.

The message discrepancy is temporary while we implement and roll out `dbt-autofix`'s enhanced compatibility detection to Fusion warnings.

Here's an example of a Fusion warning in the Studio IDE that says a package isn't compatible with Fusion but `dbt-autofix` indicates it is compatible:

```text
dbt1065: Package 'dbt_utils' requires dbt version [>=1.30,<2.0.0], but current version is 2.0.0-preview.72. This package may not be compatible with your dbt version. dbt(1065) [Ln 1, Col 1]
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
