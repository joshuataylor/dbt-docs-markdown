# Upgrading to v2 [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Available in v2

v2 is the current era of dbt, delivered through dbt v2. When you install dbt, you get dbt v2 by default. This guide walks you through upgrading a v1 project to v2.

v2 is faster and stricter, but your existing project language and DAG semantics carry over, so once you upgrade, your project works as before — just faster.

important

dbt v2 is currently available for installation in:

* [Local command line interface (CLI) tools](../../local/install-dbt.md?version=2) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [VS Code and Cursor with the dbt extension](../../install-dbt-extension.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [dbt platform environments](../upgrade-dbt-platform-version.md#dbt-v2)

Join the conversation in our Community Slack channel [`#dbt-fusion-engine`](https://getdbt.slack.com/archives/C088YCAB6GH).

## More information about dbt v2

* [About the dbt extension](../../about-dbt-extension.md)
* [Supported features matrix](../../dbt/supported-features.md)
* [Install dbt](../../local/install-dbt.md)
* [Quickstart for dbt v2](../../../guides/dbt.md?step=1)
* [dbt v2 license agreement](https://www.getdbt.com/dbt-fusion-engine-license-agreement)
* [dbt v2 changelog](https://github.com/dbt-labs/dbt/blob/main/CHANGELOG-fusion.md)

## Install dbt

Upgrading to v2 is an install step. Install dbt using `pip` to get dbt v2 for v2:

```shell
python -m pip install --pre dbt
```

For full instructions, including Homebrew, winget, and additional options, refer to [Install dbt](../../local/install-dbt.md).

## What to know before upgrading

If you have an older project that isn't ready to move to v2, or you need compatibility with existing tooling, packages, or workflows that haven't moved to v2 yet, you can stay on dbt v1.x, which remains fully supported. Over time, new capabilities will land in v2 only, so most people will eventually want to upgrade. To install or continue using v1.x, refer to [Install dbt v1.x](../../local/install-dbt.md?version=1).

This new major version is an opportunity to *strengthen the framework* by removing deprecated functionality, rationalizing confusing behavior, and providing more rigorous validation on erroneous inputs. This means that there is some work involved in preparing an existing dbt project for v2.

That work is documented below — it should be simple, straightforward, and in many cases, auto-fixable with the [`dbt-autofix`](https://github.com/dbt-labs/dbt-autofix) helper or the [agent skill](https://github.com/dbt-labs/dbt-agent-skills/tree/main/skills/dbt-migration/skills/migrating-dbt-core-to-fusion).

Test v2 parser compatibility from dbt v1.12

If you're on dbt v1.12, you can test the rust parser compatibility before fully migrating by using the opt-in [`--use-v2-parser`](../../../reference/global-configs/parsing.md#opt-in-v2-parser) flag. This delegates parsing to the v2 parser without changing any other behavior, making it a low-risk way to catch compatibility issues early.

#### Upgrade considerations

Keep in mind the following considerations during the upgrade process:

* **Manifest compatibility** — dbt v2 produces a `v12` [manifest](../../../reference/artifacts/manifest-json.md) that's compatible with dbt v1. The only differences are optional dbt v2-specific fields that only dbt v2 writes, which dbt v1 safely ignores.

  As a result, you can run dbt v2 and dbt v1 side by side. State-dependent features such as `state:modified`, `--defer`, and cross-environment `dbt docs generate` work across mixed dbt v2 and dbt v1 environments, so you can migrate to dbt v2 incrementally without breaking existing dbt v1 jobs.

State-aware orchestration is now dbt State

[dbt State](../../deploy/dbt-state-about.md) works with all engines and environments: dbt v1, the dbt platform, and dbt v2.

If you were using state-aware orchestration prior to June 1, 2026, you can continue using it. Once you start your free dbt State trial, it will be extended beyond the standard 30-day period. If the extension isn't applied to your account, contact your account team. To get started, refer to [Migrate from state-aware orchestration](../../deploy/dbt-state-migration.md).

### Supported adapters

The following adapters are supported in v2:

 BigQuery[Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

* Service Account / User Token
* Native OAuth
* External OAuth
  * [Workload Identity Federation](../../platform/manage-access/set-up-bigquery-oauth.md#set-up-bigquery-workload-identity-federation) (Microsoft Entra)
* [Required permissions](../../local/connect-data-platform/bigquery-setup.md#required-permissions)

 Databricks[Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

* Service Account / User Token
* Native OAuth

 Redshift[Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

* Username / Password
* IAM profile

 Snowflake

* Username / Password
* Native OAuth
* External OAuth
* Key pair using a modern PKCS#8 method
* MFA

 Apache Spark (CLI only)[Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

* Thrift

  * Simple Authentication and Security Layer (SASL) PLAIN
  * No SASL (NOSASL)

* Livy

  * Basic authentication (username and password)
  * When deployed on Amazon Web Services (AWS): AWS Signature Version 4
    * Supports authentication using single sign-on, service accounts, or user tokens

 DuckDB (CLI only)[Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

DuckDB does not require authentication — it runs locally on your machine.

*Note that adapter lifecycle may differ between the dbt platform and local development. An adapter can reach GA in the dbt platform before it reaches GA for local use.*

### A clean slate

v2 will not support any deprecated functionality (see the [Changes overview](../../../reference/changes-overview.md) for details):

* All [deprecation warnings](../../../reference/deprecations.md) must be resolved before upgrading to the new engine. This includes historic deprecations and [new ones as of dbt v1.10](./upgrading-to-v1.10.md#deprecation-warnings).
* Some [behavior change flags](../../../reference/global-configs/behavior-changes.md#behavior-change-flags) will be removed (generally enabled). You can no longer opt out of them using `flags:` in your `dbt_project.yml`.

### Ecosystem packages

The most popular `dbt-labs` packages (`dbt_utils`, `audit_helper`, `dbt_external_tables`, `dbt_project_evaluator`) are already compatible with v2. External packages published by organizations outside of dbt may use outdated code or incompatible features that fail to parse in v2. We're working with those package maintainers to make packages available for v2. Packages requiring an upgrade to a new release for v2 compatibility, will be documented in this upgrade guide.

## New and changed features and functionality

### Strict validation

In v1, misspelled configs, unexpected YAML keys, and invalid flags were silently ignored. In v2, dbt enforces a tightly-defined language specification at parse time and raises explicit errors for any violation, including unused config paths in `dbt_project.yml`, unknown CLI options, and duplicate config keys.

### Faster Rust parser

The v2 engine is a complete rewrite in Rust, delivering faster parse and compile times, especially on large projects. No configuration is needed; the performance improvement is automatic.

### dbt Information Schema

Similar to a database's `INFORMATION_SCHEMA`, the [dbt Information Schema](../../build/dbt-information-schema.md) is a contracted interface into the metadata for all of the resources in your dbt project.

When you use the [`--generate-info-schema`](#generating-the-information-schema) flag, dbt writes the Information Schema to `target/info_schema/` in a versioned subdirectory (for example, `target/info_schema/v1/`) as standard Parquet files. The metadata available in the schema grows with each step: parsing produces basic metadata, compiling adds column types and column-level lineage (with `--static-analysis strict`), and running or building populates runtime results.

For more information, refer to [dbt Information Schema](../../build/dbt-information-schema.md).

### Checks [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

In dbt v2, you can create [checks](../../build/checks.md) to enforce project standards (for example, all models must have a description, a public model must have an owner, and so on) at parse time, before any warehouse work runs. Write a SQL rule under the `checks/` directory, then run checks on demand with `dbt check`. Checks also run automatically with every `dbt build`. Use `--skip-checks` to bypass checks on a build.

For more information, refer to [Checks](../../build/checks.md).

### dbt Docs v2

v2 introduces [dbt Docs v2](../../build/view-documentation.md#dbt-docs-v2), a fast, modern self-hosted catalog experience built to help you understand and trust your production data. You get column-level lineage, Semantic Layer metadata, and a beautifully refreshed interface, all working smoothly on the largest projects. Under the hood, your metadata lives in efficient Parquet artifacts for faster load times and a catalog that scales as your project grows.

`dbt docs generate` compiles your project, produces the v2 Parquet artifacts, and exports a static site in a single command. `dbt docs serve` previews that site locally. Because the browser queries those artifacts directly with DuckDB-WASM, you can also host the generated files on any static file host. Column-level lineage is visible when you build with `--static-analysis strict`.

To hydrate catalog metadata (`catalog.json`) for Catalog without building the site, use the [`--write-catalog` flag](../../../reference/commands/cmd-docs.md#--write-catalog-flag) instead.

For full usage, refer to [About dbt docs commands](../../../reference/commands/cmd-docs.md?version=2).

### Model freshness and the `dbt freshness` command [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

v2 expands freshness checks to models, building on the existing support for sources. You can configure freshness thresholds on models to receive warnings or errors when the data is stale. For config options and materialization requirements, refer to [freshness](../../../reference/resource-configs/freshness.md).

Use the new [`dbt freshness`](../../../reference/commands/freshness.md) command to check all sources and models with freshness configured in a single invocation and, and to write results to a [`target/freshness.json` file](../../../reference/artifacts/freshness-json.md).

The [`dbt source freshness`](../../../reference/commands/source.md?version=2#dbt-source-freshness) command remains supported for backward compatibility, checks sources only, and continues to produce a `sources.json` file. We recommend using `dbt freshness` going forward.

### Adapters built on ADBC drivers

All v2 adapters connect to data warehouses via the [Arrow Database Connectivity (ADBC)](https://arrow.apache.org/adbc/) standard instead of Python-based adapter libraries. As a result, dbt ships as a single self-contained binary with no Python runtime required.

On first run, dbt downloads adapter drivers from the dbt Labs CDN and caches them locally. Subsequent runs work offline. For supported adapters, refer to [Supported data platforms](../../supported-data-platforms.md).

### `dbt lint` [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

v2 introduces [`dbt lint`](../../../reference/commands/lint.md), a high-performance SQL linter built into dbt. It is SQLFluff-compatible: you keep your existing .sqlfluff config and rule codes (for example, `CP01`, `RF03`). Run `dbt lint` to lint all models, or `dbt lint [FILE]` to target a specific file. Use `--fix` to auto-apply fixable violations.

### Static analysis

Unlike dbt v1.x, which only rendered Jinja-templated SQL strings into queries, v2 adds a second phase: static analysis. After rendering, the engine produces and validates a logical plan for every query in your project — without executing anything against the warehouse. This enables dialect-aware validation, column-level lineage, and precise type checking. The [`static_analysis`](../../../reference/resource-configs/static-analysis.md) config controls how strictly this is applied.

#### Baseline static analysis

Baseline mode is the default in dbt v2. It parses each model's SQL at compile time to understand its structure without a warehouse connection, catching most SQL errors while providing a smooth migration experience. In baseline mode, all findings are warnings rather than errors, so your project continues running even when the compiler flags issues.

Enable it explicitly per-model or project-wide:

```yaml
# dbt_project.yml
models:
  your_project:
    +static_analysis: baseline
```

Or pass `--static-analysis baseline` on the CLI. For details, refer to [About static analysis](../../build/about-static-analysis.md).

#### Strict static analysis

Strict mode fully resolves column types and validates references across your entire project before execution begins; nothing runs until the project is proven valid. It is required to produce [column-level lineage](../../explore/column-level-lineage.md) and unlocks additional LSP features like column go-to-definition and type checking. Strict mode requires authentication through [`dbt login`](../../../reference/commands/login.md?version=2.0); unauthenticated runs fall back to baseline.

Enable it per-model or project-wide:

```yaml
# dbt_project.yml
models:
  your_project:
    +static_analysis: strict
```

Or pass `--static-analysis strict` on the CLI (or set `DBT_STATIC_ANALYSIS=strict`). For details, refer to [static\_analysis](../../../reference/resource-configs/static-analysis.md).

Deprecated values

`static_analysis: on` and `static_analysis: unsafe` are deprecated synonyms for `strict`. Update these to `strict`; they will be removed in a future release.

### Column-level lineage

v2 tracks which source columns flow into which output columns across your entire DAG. To generate column lineage, build or compile with `--static-analysis strict`:

```shell
dbt build --static-analysis strict
```

The lineage is then visible in [dbt-docs](../../build/view-documentation.md#dbt-docs-v2). No additional configuration is needed; the site detects the presence of the lineage artifact automatically. For details, refer to [Column-level lineage](../../explore/column-level-lineage.md).

### Language server protocol (LSP)

v2 includes a built-in language server that enables IDE features for dbt SQL and YAML files, including hover information, diagnostics, go-to-definition, and column-level completions. The standalone `dbt-lsp` package is no longer published; LSP is now bundled in the `dbt` binary and used automatically by the dbt VS Code extension and Studio IDE.

Features available depend on your `static_analysis` setting: `baseline` adds syntax error detection and CTE preview; `strict` adds column go-to-definition, column lineage, and type checking. For details, refer to [About dbt LSP](../../about-dbt-lsp.md).

### Agent skills

v2 introduces [agent skills](../../dbt-ai/package-skills.md), which are reusable instructions your coding agent reads from a `SKILL.md` file. You and your team can ship skills from your own project or from a package, so everyone works from one set of conventions instead of copying files between repos.

To use agent skills, you need to:

* Include skills in your project's `skills/` directory, use a package that ships skills, or both
* Set the `ai_provider` flag in your root project to tell dbt which which coding agent you use. Supported values are `wizard`, `claude`, `openai`, `codex`, `cursor`, or `gemini` (case-insensitive).

Once both are in place, `dbt deps` installs those skills into the directory your agent reads from (such as `.claude/skills` or `.agents/skills`), and `dbt clean` removes them. If you're missing either piece, `dbt deps` installs your packages as usual and skips the skills.

For full usage info, including how to disable a skill you don't want, refer to [Installing agent skills from dbt packages](../../dbt-ai/package-skills.md).

### `dbt login`

In dbt v2, [`dbt login`](../../../reference/commands/login.md?version=2.0) enables browser-based authentication. It opens a browser window prompting you to sign in to your dbt platform account or create a free account.

Run [`dbt login status`](../../../reference/commands/login.md?version=2.0#dbt-login-status) to view your current authentication status.

### Experimental features

#### Local execution of unit tests [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

v2 introduces the [`compute`](../../../reference/resource-configs/compute.md) config for unit tests. Set your unit tests with `compute: local` and dbt runs the test with DuckDB instead of sending it to your data platform, which takes the warehouse round trip out of your development loop.

This config is experimental and requires opt-in: set `DBT_ENGINE_EXPERIMENTAL_LOCAL_UNIT_TESTS=true` in the environment where dbt runs before you use `compute: local`. For details, refer to [Run unit tests locally](../../build/unit-tests.md#run-unit-tests-locally).

#### Multi-adapter invocations

v2 supports running a single project against multiple adapters simultaneously as part of our cross-platform [dbt Mesh](../../mesh/cross-platform-mesh.md?version=2). This is experimental and requires opt-in:

```bash
export DBT_ENGINE_EXPERIMENTAL_MULTI_ADAPTER=true
```

Without this flag, dbt fails at parse time if any node in the project has an `adapter` config, even for runs that don't select that node. The gate reads config as written, not as selected, so the entire project needs the env var to parse once any model uses a non-default adapter.

### Changed functionality

When developing v2, there were opportunities to improve the dbt framework — failing earlier (when possible), fixing bugs, optimizing run order, and deprecating flags that are no longer relevant. The result is a handful of specific and nuanced changes to existing behavior.

When upgrading to v2, you should expect the following changes in functionality:

#### Parse time printing of relations will print out the full qualified name, instead of an empty string

In dbt v1, when printing the result of `get_relation()`, the parse time output for that Jinja would print `None` (the undefined object coerces to the string "None").

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

The output after `dbt parse` in dbt v1:

```text
relation: None
relation_via_api: my_db.my_schema.my_table
```

The output after `dbt parse` in v2:

```text
relation: my_db.my_schema.my_table
relation_via_api: my_db.my_schema.my_table
```

#### Deprecated flags

Deprecated flags are command-line flags (like `--models`, `--print`) that you pass to dbt commands. These are being removed in v2. This is different from:

* [Deprecation warnings](../../../reference/deprecations.md) — Features in your project code (models, YAML, macros) that need to be updated
* [Behavior change flags](../../../reference/global-configs/behavior-changes.md) — Flags in `dbt_project.yml` that let you opt in/out of new behaviors

See the [Changes overview](../../../reference/changes-overview.md) for a full comparison.

Some historic CLI flags from v1 will no longer do anything in v2. If you pass them into a dbt command in v2, the command will not error, but the flag will do nothing (and warn accordingly).

| flag name                                                                                                                                        | remediation                                                           |
| ------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------- |
| `--models` / `--model` / `-m`                                                                                                                    | Refer to [CLI flags that need changes](#cli-flags-that-need-changes). |
| `dbt seed` [`--show`](../../../reference/commands/seed.md)                                                                        | N/A                                                                   |
| [`--print` / `--no-print`](../../../reference/global-configs/print-output.md)                                                     | No action required                                                    |
| [`--printer-width`](../../../reference/global-configs/print-output.md#printer-width)                                              | No action required                                                    |
| [`--source`](../../../reference/commands/deps.md#non-hub-packages)                                                                | No action required                                                    |
| [`--record-timing-info` / `-r`](../../../reference/global-configs/record-timing-info.md)                                          | No action required                                                    |
| [`--cache-selected-only` / `--no-cache-selected-only`](../../../reference/global-configs/cache.md)                                | No action required                                                    |
| [`--clean-project-files-only` / `--no-clean-project-files-only`](../../../reference/commands/clean.md#--clean-project-files-only) | No action required                                                    |
| `--single-threaded` / `--no-single-threaded`                                                                                                     | No action required                                                    |
| `dbt source freshness` [`--output` / `-o`](../../../reference/commands/source.md?version=1.12#source-freshness-commands)          |                                                                       |
| [`--config-dir`](../../../reference/commands/debug.md)                                                                            | No action required                                                    |
| [`--resource-type` / `--exclude-resource-type`](../../../reference/global-configs/resource-type.md)                               | Refer to [CLI flags that need changes](#cli-flags-that-need-changes). |
| `--show-resource-report` / `--no-show-resource-report`                                                                                           | No action required                                                    |
| [`--log-cache-events` / `--no-log-cache-events`](../../../reference/global-configs/logs.md#logging-relational-cache-events)       | No action required                                                    |
| `--use-experimental-parser` / `--no-use-experimental-parser`                                                                                     | No action required                                                    |
| [`--empty-catalog`](../../../reference/commands/cmd-docs.md#dbt-docs-generate)                                                    |                                                                       |
| [`--compile` / `--no-compile`](../../../reference/commands/cmd-docs.md#dbt-docs-generate)                                         |                                                                       |
| `--inline-direct`                                                                                                                                | No action required                                                    |
| `--partial-parse-file-diff` / `--no-partial-parse-file-diff`                                                                                     | No action required                                                    |
| `--partial-parse-file-path`                                                                                                                      | No action required                                                    |
| `--populate-cache` / `--no-populate-cache`                                                                                                       | No action required                                                    |
| `--static-parser` / `--no-static-parser`                                                                                                         | No action required                                                    |
| `--use-fast-test-edges` / `--no-use-fast-test-edges`                                                                                             | No action required                                                    |
| `--inject-ephemeral-ctes` / `--no-inject-ephemeral-ctes`                                                                                         |                                                                       |
| [`--partial-parse` / `--no-partial-parse`](../../../reference/parsing.md#partial-parsing)                                         | Refer to [CLI flags that need changes](#cli-flags-that-need-changes). |

##### CLI flags that need changes

The following deprecated flag requires updates in your job definitions or scripts:

* **`--models` / `--model` / `-m`:** Use `--select` / `-s` instead (renamed in dbt v0.21). dbt raises an error in v2 if you use the old flags. Do not pass `--models` as the value to `-s` (for example, `dbt run -s --models`); v1 treated that as a model name, but v2 requires a valid selector.

dbt v2 job runs no longer support the `--partial-parse` and `--no-partial-parse` CLI flags. If you pass them (for example, from a dbt v1 command or script), dbt logs deprecation warning `dbt1700`. Remove these flags from your dbt v2 job commands. For more information, refer to [Deprecated flags](./upgrading-to-v2.md#deprecated-flags) in the guide to upgrading to dbt v2.

#### Conflicting package versions when a local package depends on a hub package which the root package also wants will error

If a local package depends on a hub package that the root package also wants, `dbt deps` doesn't resolve conflicting versions in dbt v1; it will install whatever the root project requests.

v2 will present an error:

```bash
error: dbt8999: Cannot combine non-exact versions: =0.8.3 and =1.1.1
```

#### Parse will fail on nonexistent macro invocations and adapter methods

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

#### Parse will fail on missing generic test

When you have an undefined generic test in your project:

```yaml

models:
  - name: dim_wizards
    data_tests:
      - does_not_exist
```

In v1, `dbt parse` passes, but `dbt compile` fails.

In v2, dbt will error out during `parse`.

#### Parse will fail on missing variable

When you have an undefined variable in your project:

```sql

select {{ var('does_not_exist') }} as my_column
```

In v1, `dbt parse` passes, but `dbt compile` fails.

In v2, dbt will error out during `parse`.

#### Stricter evaluation of duplicate docs blocks

In v1, it was possible to create scenarios with duplicate [docs blocks](../../build/documentation.md#using-docs-blocks). For example, you can have two packages with identical docs blocks referenced by an unqualified name in your dbt project. In this case, v1 would use whichever docs block is referenced without any warnings or errors.

v2 adds stricter evaluation of names of docs blocks to prevent such ambiguity. It will present an error if it detects duplicate names:

```bash
dbt found two docs with the same name: 'docs_block_title' in files: 'models/crm/_crm.md' and 'docs/crm/business_class_marketing.md'
```

To resolve this error, rename any duplicate docs blocks.

#### `dbt clean` will not delete any files in configured resource paths or files outside the project directory

In dbt v1, `dbt clean` deletes:

* Any files outside the project directory if `clean-targets` is configured with an absolute path or relative path containing `../`, though there is an opt-in config to disable this (`--clean-project-files-only` / `--no-clean-project-files-only`).
* Any files in the `asset-paths` or `doc-paths` (even though other resource paths, like `model-paths` and `seed-paths`, are restricted).

In v2, `dbt clean` will not delete any files in configured resource paths or files outside the project directory.

#### All unit tests are run first in `dbt build`

In dbt v1, the direct parents of the model being unit tested needed to exist in the warehouse to retrieve the needed column name and type information. `dbt build` runs the unit tests (and their dependent models) *in lineage order*.

In v2, `dbt build` runs *all* of the unit tests *first*, and then builds the rest of the DAG, due to built-in column name and type awareness.

#### Configuring `--threads`

dbt v1 runs with `--threads 1` by default. You can increase this number to run more nodes in parallel on the remote data platform, up to the max parallelism enabled by the DAG.

v2 handles threading differently depending on your data platform:

| Adapter        | Behavior                                                                                                                                                                                                                                                                                                                        |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Snowflake**  | dbt v2 automatically manages connection parallelism based on platform limits and backpressure. The `threads` setting acts as a maximum connection cap if set, but dbt v2 is designed to work optimally without it configured. If you're experiencing timeout or rate limit issues, setting `threads` to a lower value can help. |
| **Databricks** | dbt v2 automatically manages connection parallelism based on platform limits and backpressure. The `threads` setting acts as a maximum connection cap if set, but dbt v2 is designed to work optimally without it configured. If you're experiencing timeout or rate limit issues, setting `threads` to a lower value can help. |
| **BigQuery**   | dbt v2 respects user-set threads to manage API rate limits.<br />Setting `--threads 0` (or omitting the setting) allows dbt v2 to dynamically optimize parallelism.                                                                                                                                                             |
| **Redshift**   | dbt v2 respects user-set threads to manage concurrency limits.<br />Setting `--threads 0` (or omitting the setting) allows dbt v2 to dynamically optimize parallelism.                                                                                                                                                          |

For more information, refer to [Using threads](../../running-a-dbt-project/using-threads.md#dbt-v2-thread-optimization).

#### Continue to compile unrelated nodes after hitting a compile error

As soon as v1's `compile` encounters an error compiling one of your models, dbt stops and doesn't compile anything else.

When v2's `compile` encounters an error, it will skip nodes downstream of the one that failed to compile, but it will keep compiling the rest of the DAG (in parallel, up to the number of configured / optimal threads).

#### Seeds with extra commas don't result in extra columns

In dbt v1, if you have an additional comma on your seed, dbt creates a seed with an additional empty column.

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

In v2, it will not produce this extra column in the table resulting from `dbt seed`:

| animal |
| ------ |
| dog    |
| cat    |
| bear   |

#### Move standalone anchors under `anchors:` key

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

This move is only necessary for fragments defined outside of the main YAML structure. For more information about this new key, see [anchors](../../../reference/resource-properties/anchors.md).

#### Self-referential (recursive) YAML anchors are not supported

In v1, dbt could parse a YAML anchor that merges into an element of the same sequence it's defined on, creating a self-referential (cyclic) anchor. For example, anchoring a full `tables:` sequence and then merging that anchor into one of the sequence's own elements:

```yml
sources:
  - name: catalogue
    tables: &tables
      - name: anchor_item
        description: The first table in the sequence.
      - <<: *tables
        name: merged_item
```

This parsed successfully in v1 only because PyYAML (the YAML library dbt v1 depends on) incidentally allows self-referential anchors, a side effect of Python's own support for cyclic data structures, not an intentional YAML feature. No other major YAML implementation allows this pattern.

In v2, parsing this pattern hits a recursion limit and raises an error, so the entire properties file fails to parse. This is a deliberate limitation, not a bug. v2 does not plan to support self-referential anchors.

To resolve this, remove the self-reference. Anchor only the parts of the document that don't merge back into themselves, for example, anchor a single table mapping instead of the whole sequence:

```yml
sources:
  - name: catalogue
    tables:
      - &anchor_item_alias
        name: anchor_item
        description: The first table in the sequence.
      - <<: *anchor_item_alias
        name: merged_item
```

#### Algebraic operations in Jinja macros

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

#### Accessing custom configurations in meta

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

To access custom configurations stored under meta, use the explicit methods:

```jinja
{% set owner = config.meta_get('owner') %}
{% set has_pii = config.meta_require('pii') %}
```

For more information, see [config.meta\_get()](../../../reference/dbt-jinja-functions/config.md#configmeta_get) and [config.meta\_require()](../../../reference/dbt-jinja-functions/config.md#configmeta_require).

## Quick hits

* v2 supports exporting traces and logs in JSONL, Parquet, and OTLP formats. For details, refer to [dbt v2 telemetry and observability](../../../reference/telemetry-observability.md).
* Data tests can now run in batches (`DBT_ENGINE_BATCH_TESTS=true`) and skip redundant cached results (`DBT_ENGINE_SKIP_REDUNDANT_TESTS=true`), reducing execution overhead on large projects.
* The v2 compiler parses and type-checks [Snowflake model function](https://docs.snowflake.com/en/guides-overview-ml-functions) calls (`model!method(...)`), accepting any arguments and treating results as `VARIANT`. Cast the result to the type you need (for example, `model!predict(col)::float`).

## Package support

To determine if a package is compatible with dbt v2, visit the [dbt package hub](https://hub.getdbt.com/) and look for the dbt v2-compatible badge, or review the package's [`require-dbt-version` configuration](../../../reference/project-configs/require-dbt-version.md#pin-to-a-range).

* Packages with a `require-dbt-version` that equals or contains `2.0.0` are compatible with dbt v2. For example, `require-dbt-version: ">=1.10.0,<3.0.0"`.

  Even if a package doesn't reflect compatibility in the package hub, it may still work with v2. Work with package maintainers to track updates, and [thoroughly test packages](https://docs.getdbt.com/guides/dbt-package-compat?step=5) that aren't clearly compatible before deploying.

* Package maintainers who would like to make their package compatible with v2 can refer to the [dbt v2 package upgrade guide](../../../guides/dbt-package-compat.md) for instructions.

Fivetran package considerations:

* The Fivetran `source` and `transformation` packages have been combined into a single package.
* If you manually installed source packages like `fivetran/github_source`, you need to ensure `fivetran/github` is installed and deactivate the transformation models.

#### Package compatibility messages

Inconsistent v2 warnings and `dbt-autofix` logs

dbt v2 warnings and `dbt-autofix` logs may show different messages about package compatibility.

If you use [`dbt-autofix`](https://github.com/dbt-labs/dbt-autofix) while upgrading to v2 in the Studio IDE or dbt VS Code extension, you may see different messages about package compatibility between `dbt-autofix` and v2 warnings.

Here's why:

* dbt v2 warnings are emitted based on a package's `require-dbt-version` and whether `require-dbt-version` contains `2.0.0`.
* Some packages are already v2-compatible even though package maintainers haven't yet updated `require-dbt-version`.
* `dbt-autofix` knows about these compatible packages and will not try to upgrade a package that it knows is already compatible.

This means that even if you see a v2 warning for a package that `dbt-autofix` identifies as compatible, you don't need to change the package.

The message discrepancy is temporary while we implement and roll out `dbt-autofix`'s enhanced compatibility detection to v2 warnings.

Here's an example of a v2 warning in the Studio IDE that says a package isn't compatible with v2 but `dbt-autofix` indicates it is compatible:

```text
dbt1065: Package 'dbt_utils' requires dbt version [>=1.30,<2.0.0], but current version is 2.0.0-preview.72. This package may not be compatible with your dbt version. dbt(1065) [Ln 1, Col 1]
```

## Distributions

v2 is available in two distributions. For more information, refer to [dbt licensing](../../dbt-licensing.md).

| Distribution | Package    | Use it when                                                                                                                                   |
| ------------ | ---------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| dbt v2       | `dbt`      | The recommended v2 experience.                                                                                                                |
| dbt OSS      | `dbt-core` | Your organization has a strict requirement to use the Apache 2.0 [open-source runtime](../../local/install-dbt-v2.md). |

If you have a older project that isn’t ready to move to v2, continue using v1.x for compatibility. For new or upgraded projects, we recommend v2.
