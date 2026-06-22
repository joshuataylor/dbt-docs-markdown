# Upgrading to v2.0

dbt Core v2.0 is in alpha

dbt Core v2.0 is currently in alpha. This does not affect the Fusion in platform rollout, which continues on its existing track.

v2.0 marks a new foundation for dbt: a faster, Rust-based runtime, rebuilt from the ground up and available in two distributions. For most users, the right choice is [Fusion](https://docs.getdbt.com/docs/fusion/about-fusion.md), which extends the open-source core with SQL comprehension, column-level lineage, instant feedback, and platform-connected workflows.

For license-aware customers, we offer [dbt Core v2.0](https://docs.getdbt.com/docs/local/install-dbt.md), a distribution that includes only Apache 2.0 open-source code. Both distributions share the same project language and DAG semantics, so once you upgrade to v2.0, your existing dbt project works with either distribution.

important

The dbt Fusion engine is currently available for installation in:

* [Local command line interface (CLI) tools](https://docs.getdbt.com/docs/local/install-dbt.md?version=2) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [VS Code and Cursor with the dbt extension](https://docs.getdbt.com/docs/install-dbt-extension.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [dbt platform environments](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md#dbt-fusion-engine)

Join the conversation in our Community Slack channel [`#dbt-fusion-engine`](https://getdbt.slack.com/archives/C088YCAB6GH).

Read the [Fusion Diaries](https://github.com/dbt-labs/dbt-core/discussions/categories/announcements?discussions_q=is:open+diaries+category:Announcements) for the latest updates.

<!-- -->

## More information about Fusion[​](#more-information-about-fusion "Direct link to More information about Fusion")

Fusion marks a significant update to dbt. While many of the workflows you've grown accustomed to remain unchanged, there are a lot of new ideas, and a lot of old ones going away. The following is a list of the full scope of our current release of the Fusion engine, including implementation, installation, deprecations, and limitations:

* [About the dbt Fusion engine](https://docs.getdbt.com/docs/fusion/about-fusion.md)
* [About the dbt extension](https://docs.getdbt.com/docs/about-dbt-extension.md)
* [New concepts in Fusion](https://docs.getdbt.com/docs/fusion/new-concepts.md)
* [Supported features matrix](https://docs.getdbt.com/docs/fusion/supported-features.md)
* [Installing Fusion CLI](https://docs.getdbt.com/docs/local/install-dbt.md?version=2)
* [Installing VS Code extension](https://docs.getdbt.com/docs/install-dbt-extension.md)
* [Fusion release track](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md#dbt-fusion-engine)
* [Quickstart for Fusion](https://docs.getdbt.com/guides/fusion.md?step=1)
* [Upgrade guide](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md)
* [Fusion license agreement](https://www.getdbt.com/dbt-fusion-engine-license-agreement)

## Distributions[​](#distributions "Direct link to Distributions")

Two dbt distributions build on the Fusion runtime:

| Distribution      | Install                                                                                                   | License     | What you get                                                                                      |
| ----------------- | --------------------------------------------------------------------------------------------------------- | ----------- | ------------------------------------------------------------------------------------------------- |
| **Fusion**        | pip, brew, winget, CDN — [see all options](https://docs.getdbt.com/docs/local/install-dbt.md?version=2.0) | Proprietary | dbt Core v2.0 foundation plus Fusion features: SQL comprehension, column-level lineage, and more. |
| **dbt Core v2.0** | pip, brew                                                                                                 | Apache 2.0  | Open-source Rust-based runtime: Faster parsing and execution.                                     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Installation[​](#installation "Direct link to Installation")

For most users, the recommended path is to install Fusion as it includes all of dbt Core v2.0 plus the robust Fusion feature set.

### Fusion[​](#fusion "Direct link to Fusion")

Install Fusion using pip, or see all [installation options](https://docs.getdbt.com/docs/local/install-dbt.md?version=2.0) (brew, winget, CDN):

```shell
python -m pip install --pre dbt
```

### dbt Core v2.0[​](#dbt-core-v20 "Direct link to dbt Core v2.0")

If you specifically need the open-source distribution of v2, install dbt Core. During alpha, you must target either the pre-release version or an explicit pin. You can copy the following commands to install the alpha and immediately update to the most recent version:

Pre-release version:

```shell
python -m pip install --pre dbt-core
```

Explicit pin:

```shell
python -m pip install dbt-core==2.0.0-alpha.1
```

## What to know before upgrading[​](#what-to-know-before-upgrading "Direct link to What to know before upgrading")

This new major version is an opportunity to *strengthen the framework* by removing deprecated functionality, rationalizing confusing behavior, and providing more rigorous validation on erroneous inputs. This means that there is some work involved in preparing an existing dbt project for v2.

That work is documented below — it should be simple, straightforward, and in many cases, auto-fixable with the [`dbt-autofix`](https://github.com/dbt-labs/dbt-autofix) helper or the [agent skill](https://github.com/dbt-labs/dbt-agent-skills/tree/main/skills/dbt-migration/skills/migrating-dbt-core-to-fusion).

Test Fusion parser compatibility from dbt Core v1.12

If you're on dbt Core v1.12, you can test Fusion parser compatibility before fully migrating by using the opt-in [`--use-v2-parser`](https://docs.getdbt.com/reference/global-configs/parsing.md#opt-in-v2-parser) flag. This delegates parsing to the Fusion parser without changing any other behavior, making it a low-risk way to catch compatibility issues early.

#### Upgrade considerations[​](#upgrade-considerations "Direct link to Upgrade considerations")

Keep in mind the following considerations during the upgrade process:

* **Manifest incompatibility** — Fusion is backwards-compatible and can read dbt Core [manifests](https://docs.getdbt.com/reference/artifacts/manifest-json.md). However, dbt Core isn't forward-compatible and can't read Fusion manifests. Fusion produces a `v20` manifest, while the latest version of dbt Core still produces a `v12` manifest.

  As a result, mixing dbt Core and Fusion manifests across environments breaks cross-environment features. To avoid this, use `state:modified`, `--defer`, and cross-environment `dbt docs generate` only after *all* environments are running the latest Fusion version. Using these features before all environments are on Fusion may cause errors and failures.

* **State-aware orchestration** — If using [state-aware orchestration](https://docs.getdbt.com/docs/deploy/state-aware-about.md), dbt doesn't detect a change if a table or view is dropped outside of dbt, as the cache is unique to each dbt platform environment. This means state-aware orchestration will not rebuild that model until either there is new data or a change in the code that the model uses.

  * **Workarounds:**

    * Use the **Clear cache** button on the target Environment page to force a full rebuild (acts like a reset), or
    * Temporarily disable state-aware orchestration for the job and rerun it.

  State-aware orchestration is now dbt State

  [dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md) works with all engines and environments: dbt Core, the dbt platform, and dbt Fusion engine.

  If you're using state-aware orchestration prior to June 1, 2026, you can continue using it. Existing state-aware orchestration customers automatically receive a 90-day trial of dbt State. To get started, refer to [Migrate from state-aware orchestration](https://docs.getdbt.com/docs/deploy/dbt-state-migration.md).

### Supported adapters[​](#supported-adapters "Direct link to Supported adapters")

The following adapters are supported in v2.0:

 BigQuery[Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

* Service Account / User Token
* Native OAuth
* External OAuth
  <!-- -->
  * [Workload Identity Federation](https://docs.getdbt.com/docs/platform/manage-access/set-up-bigquery-oauth.md#set-up-bigquery-workload-identity-federation) (Microsoft Entra)
* [Required permissions](https://docs.getdbt.com/docs/local/connect-data-platform/bigquery-setup.md#required-permissions)

 Databricks[Private preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

* Service Account / User Token
* Native OAuth

 Redshift[Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

* Username / Password
* IAM profile

 Salesforce Data 360

* JSON Web Token (JWT) bearer authentication

 Snowflake

* Username / Password
* Native OAuth
* External OAuth
* Key pair using a modern PKCS#8 method
* MFA

 Apache Spark (Fusion CLI only)[Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

* Thrift

  <!-- -->

  * Simple Authentication and Security Layer (SASL) PLAIN
  * No SASL (NOSASL)

* Livy

  <!-- -->

  * Basic authentication (username and password)
  * When deployed on Amazon Web Services (AWS): AWS Signature Version 4
    <!-- -->
    * Supports authentication using single sign-on, service accounts, or user tokens

 DuckDB (Fusion CLI only)[Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

DuckDB does not require authentication — it runs locally on your machine.

### A clean slate[​](#a-clean-slate "Direct link to A clean slate")

v2 will not support any deprecated functionality (see the [Changes overview](https://docs.getdbt.com/reference/changes-overview.md) for details):

* All [deprecation warnings](https://docs.getdbt.com/reference/deprecations.md) must be resolved before upgrading to the new engine. This includes historic deprecations and [new ones as of dbt Core v1.10](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v1.10.md#deprecation-warnings).
* Some [behavior change flags](https://docs.getdbt.com/reference/global-configs/behavior-changes.md#behavior-change-flags) will be removed (generally enabled). You can no longer opt out of them using `flags:` in your `dbt_project.yml`.

### Ecosystem packages[​](#ecosystem-packages "Direct link to Ecosystem packages")

The most popular `dbt-labs` packages (`dbt_utils`, `audit_helper`, `dbt_external_tables`, `dbt_project_evaluator`) are already compatible with Fusion. External packages published by organizations outside of dbt may use outdated code or incompatible features that fail to parse with the new Fusion engine. We're working with those package maintainers to make packages available for Fusion. Packages requiring an upgrade to a new release for Fusion compatibility, will be documented in this upgrade guide.

## New and changed features and functionality[​](#new-and-changed-features-and-functionality "Direct link to New and changed features and functionality")

### Changed functionality[​](#changed-functionality "Direct link to Changed functionality")

When developing v2, there were opportunities to improve the dbt framework — failing earlier (when possible), fixing bugs, optimizing run order, and deprecating flags that are no longer relevant. The result is a handful of specific and nuanced changes to existing behavior.

When upgrading to v2, you should expect the following changes in functionality:

#### Parse time printing of relations will print out the full qualified name, instead of an empty string[​](#parse-time-printing-of-relations-will-print-out-the-full-qualified-name-instead-of-an-empty-string "Direct link to Parse time printing of relations will print out the full qualified name, instead of an empty string")

In dbt Core v1, when printing the result of `get_relation()`, the parse time output for that Jinja would print `None` (the undefined object coerces to the string "None").

In v2, to help with intelligent batching of `get_relation()` calls (and significantly speed up `dbt compile`), dbt needs to construct a relation object with the fully qualified name resolved at parse time for the `get_relation()` adapter call.

Constructing a relation object with the fully qualified name in v2 produces different behavior than v1 in `print()`, `log()`, or any Jinja macro that outputs to `stdout` or `stderr` at parse time.

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

The output after `dbt parse` in v2:

```text
relation: my_db.my_schema.my_table
relation_via_api: my_db.my_schema.my_table
```

#### Deprecated flags[​](#deprecated-flags "Direct link to Deprecated flags")

Deprecated flags are command-line flags (like `--models`, `--print`) that you pass to dbt commands. These are being removed in v2. This is different from:

* [Deprecation warnings](https://docs.getdbt.com/reference/deprecations.md) — Features in your project code (models, YAML, macros) that need to be updated
* [Behavior change flags](https://docs.getdbt.com/reference/global-configs/behavior-changes.md) — Flags in `dbt_project.yml` that let you opt in/out of new behaviors

See the [Changes overview](https://docs.getdbt.com/reference/changes-overview.md) for a full comparison.

Some historic CLI flags from v1 will no longer do anything in v2. If you pass them into a dbt command in v2, the command will not error, but the flag will do nothing (and warn accordingly).

| flag name                                                                                                                                        | remediation                                                           |
| ------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------- |
| `--models` / `--model` / `-m`                                                                                                                    | Refer to [CLI flags that need changes](#cli-flags-that-need-changes). |
| `dbt seed` [`--show`](https://docs.getdbt.com/reference/commands/seed.md)                                                                        | N/A                                                                   |
| [`--print` / `--no-print`](https://docs.getdbt.com/reference/global-configs/print-output.md)                                                     | No action required                                                    |
| [`--printer-width`](https://docs.getdbt.com/reference/global-configs/print-output.md#printer-width)                                              | No action required                                                    |
| [`--source`](https://docs.getdbt.com/reference/commands/deps.md#non-hub-packages)                                                                | No action required                                                    |
| [`--record-timing-info` / `-r`](https://docs.getdbt.com/reference/global-configs/record-timing-info.md)                                          | No action required                                                    |
| [`--cache-selected-only` / `--no-cache-selected-only`](https://docs.getdbt.com/reference/global-configs/cache.md)                                | No action required                                                    |
| [`--clean-project-files-only` / `--no-clean-project-files-only`](https://docs.getdbt.com/reference/commands/clean.md#--clean-project-files-only) | No action required                                                    |
| `--single-threaded` / `--no-single-threaded`                                                                                                     | No action required                                                    |
| `dbt source freshness` [`--output` / `-o`](https://docs.getdbt.com/docs/deploy/source-freshness.md)                                              |                                                                       |
| [`--config-dir`](https://docs.getdbt.com/reference/commands/debug.md)                                                                            | No action required                                                    |
| [`--resource-type` / `--exclude-resource-type`](https://docs.getdbt.com/reference/global-configs/resource-type.md)                               | Refer to [CLI flags that need changes](#cli-flags-that-need-changes). |
| `--show-resource-report` / `--no-show-resource-report`                                                                                           | No action required                                                    |
| [`--log-cache-events` / `--no-log-cache-events`](https://docs.getdbt.com/reference/global-configs/logs.md#logging-relational-cache-events)       | No action required                                                    |
| `--use-experimental-parser` / `--no-use-experimental-parser`                                                                                     | No action required                                                    |
| [`--empty-catalog`](https://docs.getdbt.com/reference/commands/cmd-docs.md#dbt-docs-generate)                                                    |                                                                       |
| [`--compile` / `--no-compile`](https://docs.getdbt.com/reference/commands/cmd-docs.md#dbt-docs-generate)                                         |                                                                       |
| `--inline-direct`                                                                                                                                | No action required                                                    |
| `--partial-parse-file-diff` / `--no-partial-parse-file-diff`                                                                                     | No action required                                                    |
| `--partial-parse-file-path`                                                                                                                      | No action required                                                    |
| `--populate-cache` / `--no-populate-cache`                                                                                                       | No action required                                                    |
| `--static-parser` / `--no-static-parser`                                                                                                         | No action required                                                    |
| `--use-fast-test-edges` / `--no-use-fast-test-edges`                                                                                             | No action required                                                    |
| [`--introspect` / `--no-introspect`](https://docs.getdbt.com/reference/commands/compile.md#introspective-queries)                                | No action required                                                    |
| `--inject-ephemeral-ctes` / `--no-inject-ephemeral-ctes`                                                                                         |                                                                       |
| [`--partial-parse` / `--no-partial-parse`](https://docs.getdbt.com/reference/parsing.md#partial-parsing)                                         | Refer to [CLI flags that need changes](#cli-flags-that-need-changes). |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

##### CLI flags that need changes[​](#cli-flags-that-need-changes "Direct link to CLI flags that need changes")

The following deprecated flags require updates in your job definitions or scripts:

* **`--models` / `--model` / `-m`:** Use `--select` / `-s` instead (renamed in dbt Core v0.21). dbt raises an error in v2 if you use the old flags. Do not pass `--models` as the value to `-s` (for example, `dbt run -s --models`); v1 treated that as a model name, but v2 requires a valid selector.

* **`--resource-type` / `--exclude-resource-type`:** Use `--resource-types` / `--exclude-resource-types`. For more information, see [Resource type flags](https://docs.getdbt.com/reference/global-configs/resource-type.md).

Fusion job runs no longer support the `--partial-parse` and `--no-partial-parse` CLI flags. If you pass them (for example, from a dbt Core command or script), dbt logs deprecation warning `dbt1700`. Remove these flags from your Fusion job commands. For more information, refer to [Deprecated flags](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md#deprecated-flags) in the guide to upgrading to the dbt Fusion engine.

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

In v1, `dbt parse` passes, but `dbt compile` fails.

In v2, dbt will error out during `parse`.

#### Parse will fail on missing generic test[​](#parse-will-fail-on-missing-generic-test "Direct link to Parse will fail on missing generic test")

When you have an undefined generic test in your project:

```yaml

models:
  - name: dim_wizards
    data_tests:
      - does_not_exist
```

In v1, `dbt parse` passes, but `dbt compile` fails.

In v2, dbt will error out during `parse`.

#### Parse will fail on missing variable[​](#parse-will-fail-on-missing-variable "Direct link to Parse will fail on missing variable")

When you have an undefined variable in your project:

```sql

select {{ var('does_not_exist') }} as my_column
```

In v1, `dbt parse` passes, but `dbt compile` fails.

In v2, dbt will error out during `parse`.

#### Stricter evaluation of duplicate docs blocks[​](#stricter-evaluation-of-duplicate-docs-blocks "Direct link to Stricter evaluation of duplicate docs blocks")

In v1, it was possible to create scenarios with duplicate [docs blocks](https://docs.getdbt.com/docs/build/documentation.md#using-docs-blocks). For example, you can have two packages with identical docs blocks referenced by an unqualified name in your dbt project. In this case, v1 would use whichever docs block is referenced without any warnings or errors.

Fusion adds stricter evaluation of names of docs blocks to prevent such ambiguity. It will present an error if it detects duplicate names:

```bash
dbt found two docs with the same name: 'docs_block_title' in files: 'models/crm/_crm.md' and 'docs/crm/business_class_marketing.md'
```

To resolve this error, rename any duplicate docs blocks.

#### `dbt clean` will not delete any files in configured resource paths or files outside the project directory[​](#dbt-clean-will-not-delete-any-files-in-configured-resource-paths-or-files-outside-the-project-directory "Direct link to dbt-clean-will-not-delete-any-files-in-configured-resource-paths-or-files-outside-the-project-directory")

In dbt Core v1, `dbt clean` deletes:

* Any files outside the project directory if `clean-targets` is configured with an absolute path or relative path containing `../`, though there is an opt-in config to disable this (`--clean-project-files-only` / `--no-clean-project-files-only`).
* Any files in the `asset-paths` or `doc-paths` (even though other resource paths, like `model-paths` and `seed-paths`, are restricted).

In v2, `dbt clean` will not delete any files in configured resource paths or files outside the project directory.

#### All unit tests are run first in `dbt build`[​](#all-unit-tests-are-run-first-in-dbt-build "Direct link to all-unit-tests-are-run-first-in-dbt-build")

In dbt Core v1, the direct parents of the model being unit tested needed to exist in the warehouse to retrieve the needed column name and type information. `dbt build` runs the unit tests (and their dependent models) *in lineage order*.

In v2, `dbt build` runs *all* of the unit tests *first*, and then builds the rest of the DAG, due to built-in column name and type awareness.

#### Configuring `--threads`[​](#configuring---threads "Direct link to configuring---threads")

dbt Core v1 runs with `--threads 1` by default. You can increase this number to run more nodes in parallel on the remote data platform, up to the max parallelism enabled by the DAG.

v2 handles threading differently depending on your data platform:

| Adapter        | Behavior                                                                                                                                                                                                                                                                                                                        |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Snowflake**  | Fusion automatically manages connection parallelism based on platform limits and backpressure. The `threads` setting acts as a maximum connection cap if set, but Fusion is designed to work optimally without it configured. If you're experiencing timeout or rate limit issues, setting `threads` to a lower value can help. |
| **Databricks** | Fusion automatically manages connection parallelism based on platform limits and backpressure. The `threads` setting acts as a maximum connection cap if set, but Fusion is designed to work optimally without it configured. If you're experiencing timeout or rate limit issues, setting `threads` to a lower value can help. |
| **BigQuery**   | Fusion respects user-set threads to manage API rate limits.<br />Setting `--threads 0` (or omitting the setting) allows Fusion to dynamically optimize parallelism.                                                                                                                                                             |
| **Redshift**   | Fusion respects user-set threads to manage concurrency limits.<br />Setting `--threads 0` (or omitting the setting) allows Fusion to dynamically optimize parallelism.                                                                                                                                                          |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

For more information, refer to [Using threads](https://docs.getdbt.com/docs/running-a-dbt-project/using-threads.md#fusion-engine-thread-optimization).

#### Continue to compile unrelated nodes after hitting a compile error[​](#continue-to-compile-unrelated-nodes-after-hitting-a-compile-error "Direct link to Continue to compile unrelated nodes after hitting a compile error")

As soon as dbt Core v1 `compile` encounters an error compiling one of your models, dbt stops and doesn't compile anything else.

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

In v2, it will not produce this extra column in the table resulting from `dbt seed`:

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

In v1, you can set algebraic functions in the return function of a Jinja macro:

```jinja
{% macro my_macro() %}

return('xyz') + 'abc'

{% endmacro %}
```

This is no longer supported in v2 and will emit a warning:

```bash
[warning] [JinjaTopLevelReturn (dbt1508)]: return is not at the top level of the block.
Its value is final and cannot be modified by surrounding expressions.
Example: return(0) + 1. The + 1 is ignored and the macro returns 0.
```

This is not a common use case and there is no deprecation warning for this behavior in v1. The supported format is:

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
