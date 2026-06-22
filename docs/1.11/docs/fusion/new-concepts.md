# A new concept: static analysis

important

The dbt Fusion engine is currently available for installation in:

* [Local command line interface (CLI) tools](https://docs.getdbt.com/docs/local/install-dbt.md?version=2) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [VS Code and Cursor with the dbt extension](https://docs.getdbt.com/docs/install-dbt-extension.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [dbt platform environments](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md#dbt-fusion-engine)

Join the conversation in our Community Slack channel [`#dbt-fusion-engine`](https://getdbt.slack.com/archives/C088YCAB6GH).

Read the [Fusion Diaries](https://github.com/dbt-labs/dbt-core/discussions/categories/announcements?discussions_q=is:open+diaries+category:Announcements) for the latest updates.

The dbt Fusion engine [fully comprehends your project's SQL](https://docs.getdbt.com/blog/the-levels-of-sql-comprehension), enabling advanced capabilities like dialect-aware validation and precise column-level lineage.

It can do this because its compilation step is more comprehensive than that of the dbt Core engine. When dbt Core referred to *compilation*, it only meant *rendering* — converting Jinja-templated strings into a SQL query to send to a database.

dbt Fusion engine can also render Jinja, but then it completes a second phase: *static analysis*, producing and validating a logical plan for every rendered query in the project. This step is the cornerstone of Fusion's new capabilities.

| Step                                        | dbt Core v1 engine | dbt Core v2 (alpha) | dbt Fusion engine |
| ------------------------------------------- | ------------------ | ------------------- | ----------------- |
| Render Jinja into SQL                       | ✅                 | ✅                  | ✅                |
| Produce and statically analyze logical plan | ❌                 | ✅ (Fusion + login) | ✅                |
| Run rendered SQL                            | ✅                 | ✅                  | ✅                |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Principles of static analysis[​](#principles-of-static-analysis "Direct link to Principles of static analysis")

The software engineering concept of [static analysis](https://en.wikipedia.org/wiki/Static_program_analysis) describes checks that can be done on code before it runs (static == not running).

The most rigorous static analysis means you can trust that if the analysis succeeds, the code will run in production without compilation errors.

Less strict static analysis also surfaces helpful information to developers as they work. There's no free lunch—what you gain in responsiveness you lose in correctness guarantees.

The dbt Fusion engine uses the [`static_analysis`](https://docs.getdbt.com/reference/resource-configs/static-analysis.md) config to help you control how it performs static analysis for your models.

The dbt Fusion engine is unique in that it can statically analyze not just a single model in isolation, but every query from one end of your DAG to the other. Even your database can only validate the query in front of it! Concepts like [information flow theory](https://roundup.getdbt.com/i/156064124/beyond-cll-information-flow-theory-and-metadata-propagation) — although not incorporated into the dbt platform [yet](https://www.getdbt.com/blog/where-we-re-headed-with-the-dbt-fusion-engine) — rely on stable inputs and the ability to trace columns DAG-wide.

### Baseline mode: A smooth transition from dbt Core[​](#baseline-mode-a-smooth-transition-from-dbt-core "Direct link to Baseline mode: A smooth transition from dbt Core")

The dbt Fusion engine defaults to `static_analysis: baseline` mode, inspired by similar type-checking and linting tools like [TypeScript's migration approach](https://www.typescriptlang.org/docs/handbook/migrating-from-javascript.html), [basedpyright's baseline feature](https://docs.basedpyright.com/latest/benefits-over-pyright/baseline/), and [Pydantic's strict/lax modes](https://docs.pydantic.dev/latest/why/#strict-lax).

The philosophy behind the above-mentioned tools and Fusion's baseline mode is:

* **Smooth transition**: Provide a familiar first-time experience for users coming from dbt Core.
* **Incremental opt-in**: Offer a clear pathway to adopt more Fusion features over time.
* **Pragmatic validation**: Catch most SQL errors without requiring a complete project overhaul.

Use this style of gradual typing to start with lightweight validation, then incrementally adopt strict guarantees as your project is ready.

#### What baseline mode changes[​](#what-baseline-mode-changes "Direct link to What baseline mode changes")

Baseline mode introduces several fundamental behavior changes compared to the previous binary (off/on) approach:

* **No downloading of remote schemas** — Baseline mode does not fetch schemas from the warehouse.
* **Unit tests work without strict mode** — Previously, unit tests required static analysis to be fully on. In baseline mode, they work out of the box.
* **No unsafe introspection warnings** — We no longer warn about unsafe introspection, though we'd still love to help you assess it in the future.

The following table shows how baseline mode expands what's available without requiring strict mode.

### LSP feature comparison[​](#lsp-feature-comparison "Direct link to LSP feature comparison")

Baseline mode unlocks a meaningful set of features without requiring strict mode. We're also investing in moving more features into baseline over time.

VS Code extension features by static analysis configuration:

✅ = Available | ❌ = Not available

| Feature                                        | off | baseline | strict |
| ---------------------------------------------- | --- | -------- | ------ |
| Go-to-definition/reference (except columns)    | ✅  | ✅       | ✅     |
| Table lineage                                  | ✅  | ✅       | ✅     |
| YAML validation                                | ✅  | ✅       | ✅     |
| Render + preview SQL                           | ✅  | ✅       | ✅     |
| Unit tests                                     | ✅  | ✅       | ✅     |
| Detect syntax errors                           | ❌  | ✅       | ✅     |
| Preview CTE results                            | ❌  | ✅       | ✅     |
| Go-to-definition/reference (columns)           | ❌  | ❌       | ✅     |
| Automatic refactor column names                | ❌  | ❌       | ✅     |
| Rich column lineage                            | ❌  | ❌       | ✅     |
| Detect data type and function signature errors | ❌  | ❌       | ✅     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

* dbt VS Code extension features in this table are available to all users for 14 days.
* After the 14-day period, sign in or register for a dbt platform account from the dbt VS Code extension to keep using advanced capabilities.
* Unregistered users can continue using core editing and build workflows without signing in.
* Existing registered dbt VS Code extension users keep access to registration-required features automatically.

Supported Snowflake functions

To check out which Snowflake functions are supported in Fusion in `strict` mode, refer to [Snowflake function support](https://docs.getdbt.com/reference/resource-configs/snowflake-function-support.md)

CodeLens visibility

The VS Code extension and Studio IDE provide CodeLens even when static analysis is off, giving you visibility into which models have static analysis disabled and why.

Ultimately, we want everyone developing in strict mode for maximum guarantees. We acknowledge this isn't a change that can happen overnight — baseline exists to smooth the transition. Many planned features (like local compute) require strict mode. We're also exploring inferring column types on your behalf, which would enable more functionality in baseline mode without requiring you to manually provide type information.

### Introspection handling in baseline mode[​](#introspection-handling-in-baseline-mode "Direct link to Introspection handling in baseline mode")

In `baseline` mode, all static analysis findings are warnings, not errors — your project can continue running even when the compiler flags invalid or problematic SQL. This section is a good example of why that design exists.

Previously, with `strict` mode, the system assumed local schemas of your compiled models would be available. In `baseline` mode, we can no longer assume the full local schema is available and complete, so `baseline` uses the remote database as the source of truth — similar to dbt Core.

The practical result is that the Fusion compiler may sometimes flag incorrect queries that result from introspective queries that come back empty. If you encounter this, you can:

1. Ignore the warning
2. Build the model locally
3. (Coming soon) Use `warn_error_options` to disable the warning

For example, consider this query using the `dbt_utils.unpivot` macro:

```sql
select * from (
{{
    dbt_utils.unpivot(
        relation=ref('example_model'),
        cast_to='integer',
        exclude=['order_id', 'customer_id'],
        field_name='product_type',
        value_name='quantity'
    )
}}
)
```

If the introspection query fails or returns no results, this renders to:

```sql
select * from (

)
```

This is invalid SQL. In `baseline` mode, Fusion displays a warning so your project can continue running while still alerting you to the issue:

```bash
dbt0101: no viable alternative at input '(
    
)'
  --> models/example_model.sql:17:1
```

#### Migration scenarios[​](#migration-scenarios "Direct link to Migration scenarios")

Migrating to Fusion can involve more than moving YAML around. Some scenarios that can make migration more involved include:

1. **Limited access to sources**: You don't have access to all the sources and models of a large dbt project.
2. **Intricate Jinja workflows**: Your project uses post-hooks and introspection extensively.
3. **Package compatibility**: Your project depends on packages that aren't yet Fusion-compatible.
4. **Unsupported SQL features**: Your models or sources use advanced data types (`STRUCT`, `ARRAY`, `GEOGRAPHY`) or built-in functions (`AI.PREDICT`, `JSON_FLATTEN`, `st_pointfromgeohash`) not yet supported by the dbt Fusion engine.

Setting `static_analysis` to `baseline` mode lets you start using Fusion immediately while you address these scenarios incrementally. As you resolve compatibility issues, you can opt specific models or your entire project into `strict` mode for maximum validation guarantees.

## Recapping the differences between engines[​](#recapping-the-differences-between-engines "Direct link to Recapping the differences between engines")

dbt Core v1 and [dbt Core v2](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md) (currently in alpha):

* Renders and runs models one at a time.
* Never runs static analysis.

The dbt Fusion engine (baseline mode — default):

* Statically analyzes all models, catching most SQL errors while providing a familiar migration experience.

The dbt Fusion engine (strict mode):

* Renders and statically analyzes all models before execution begins.
* Guarantees nothing runs until the entire project is proven valid.
* Parses `CREATE FUNCTION` in [`sql_header`](https://docs.getdbt.com/reference/resource-configs/sql_header.md) and in [`on-run-start`](https://docs.getdbt.com/reference/project-configs/on-run-start-on-run-end.md) hooks, then registers those UDFs so strict compilation can resolve calls to them. `baseline` and `off` don't register UDFs this way. See [User-defined functions (UDFs) in `strict` mode](https://docs.getdbt.com/reference/resource-configs/static-analysis.md#user-defined-functions-udfs-in-strict-mode).

## Configuring `static_analysis`[​](#configuring-static_analysis "Direct link to configuring-static_analysis")

You can modify the way static analysis is applied for specific models in your project. The static analysis configuration cascades from most strict to least strict. Going downstream in your lineage, a model can keep the same mode or relax it — it can't be stricter than its parent.

Setting `static_analysis: strict` on a model does not automatically set `strict` for downstream models; they keep the project default unless you set them explicitly. For rules and examples, refer to [strict mode inheritance](#strict-mode-inheritance) and [How static analysis modes cascade](https://docs.getdbt.com/reference/resource-configs/static-analysis.md#how-static-analysis-modes-cascade).

The [`static_analysis`](https://docs.getdbt.com/reference/resource-configs/static-analysis.md) config options are:

* `baseline` (default): Statically analyze SQL. This is the recommended starting point for users transitioning from dbt Core, providing a smooth migration experience while still catching most SQL errors.
* `strict` (previously `on`): Statically analyze all SQL before execution begins. Use this for maximum validation guarantees — nothing runs until the entire project is proven valid.
* `off`: Skip SQL analysis on this model and its descendants.

<!-- -->

Deprecated values

The `on` and `unsafe` values are deprecated and will be removed in May 2026. Use `strict` instead.

When you disable static analysis, features of the VS Code extension which depend on SQL comprehension will be unavailable.

The best place to configure `static_analysis` is as a config on an individual model or group of models. As a debugging aid, you can also use the [`--static-analysis strict`](https://docs.getdbt.com/reference/global-configs/static-analysis-flag.md) or `--static-analysis off` CLI flags to override all model-level configuration.

### Incrementally adopting strict mode[​](#incrementally-adopting-strict-mode "Direct link to Incrementally adopting strict mode")

Once you're comfortable with Fusion in baseline mode, you can incrementally opt models or directories into `strict` mode:

dbt\_project.yml

```yml
name: jaffle_shop

models:
  jaffle_shop:
    # Start with strict analysis on your cleanest models
    staging:
      +static_analysis: strict
    # Keep baseline for models that need more work
    marts:
      +static_analysis: baseline
```

#### strict mode inheritance[​](#strict-mode-inheritance "Direct link to strict mode inheritance")

Unlike `baseline` or `off`, `strict` mode doesn't propagate to downstream models. If you configure a model as `strict`, its downstream models won't inherit `strict` mode unless you set them explicitly. To make all models `strict`, you must set `+static_analysis: strict` on root models first, or use the project-wide config in the next section at the project level.

For example, in A → B → C with a default of `baseline`, configuring A (a root node) as `strict` makes only A `strict` — B and C remain baseline unless configured. To make the full chain `strict`, set `+static_analysis: strict` on each relevant model or group, or use a project-wide setting.

This approach lets you gain the benefits of strict validation where possible while keeping the flexibility of baseline analysis for models that aren't yet compatible.

Refer to [CLI options](https://docs.getdbt.com/reference/global-configs/command-line-options.md) and [Configurations and properties](https://docs.getdbt.com/reference/configs-and-properties.md) to learn more about configs.

### Example configurations[​](#example-configurations "Direct link to Example configurations")

##### Configure strict for the entire project[​](#configure-strict-for-the-entire-project "Direct link to Configure strict for the entire project")

Many teams want to enable `strict` mode for the whole project and all packages. You can do this by setting `+static_analysis: strict` under each resource type in `dbt_project.yml` for your project name (and for any package names if you want those to be strict too):

dbt\_project.yml

```yaml
models:
  my_project:
    +static_analysis: strict

seeds:
  my_project:
    +static_analysis: strict

snapshots:
  my_project:
    +static_analysis: strict

tests:
  my_project:
    +static_analysis: strict

unit_tests:
  my_project:
    +static_analysis: strict

sources:
  my_project:
    +static_analysis: strict

analyses:
  my_project:
    +static_analysis: strict
```

Use your project name in place of `my_project` — that's the same value as the `name:` key at the top of `dbt_project.yml` (for example, `jaffle_shop`). To apply `strict` to a package as well, add another entry under each resource type using the package name as the key; for example, under `models:` add `your_package_name:` with `+static_analysis: strict` beneath it.

##### Disable static analysis for all models in a package:[​](#disable-static-analysis-for-all-models-in-a-package "Direct link to Disable static analysis for all models in a package:")

dbt\_project.yml

```yml
name: jaffle_shop

models:
  jaffle_shop: 
    marts:
      +materialized: table
  
  a_package_with_introspective_queries:
    +static_analysis: off
```

##### Disable static analysis in YAML:[​](#disable-static-analysis-in-yaml "Direct link to Disable static analysis in YAML:")

models/my\_udf\_using\_model.yml

```yml
models:
  - name: model_with_static_analysis_off
    config:
      static_analysis: off
```

##### Disable static analysis for a model using a custom UDF:[​](#disable-static-analysis-for-a-model-using-a-custom-udf "Direct link to Disable static analysis for a model using a custom UDF:")

models/my\_udf\_using\_model.sql

```sql
{{ config(static_analysis='off') }}

select 
  user_id,
  my_cool_udf(ip_address) as cleaned_ip
from {{ ref('my_model') }}
```

### When should I turn static analysis `off`?[​](#when-should-i-turn-static-analysis-off "Direct link to when-should-i-turn-static-analysis-off")

With baseline mode enabled by default, static analysis is less likely to block your runs. You should only disable it if the dbt Fusion engine cannot parse SQL that is valid for your database of choice.

This is a very rare occurrence. If you encounter this situation, please [open an issue](https://github.com/dbt-labs/dbt-fusion/issues) with an example of the failing SQL so we can update our parsers.

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

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
