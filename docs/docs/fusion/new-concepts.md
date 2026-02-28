# New concepts

<!-- -->

The dbt Fusion engine [fully comprehends your project's SQL](https://docs.getdbt.com/blog/the-levels-of-sql-comprehension), enabling advanced capabilities like dialect-aware validation and precise column-level lineage.

It can do this because its compilation step is more comprehensive than that of the dbt Core engine. When dbt Core referred to *compilation*, it only meant *rendering* — converting Jinja-templated strings into a SQL query to send to a database.

The dbt Fusion engine can also render Jinja, but then it completes a second phase: producing and validating with *static analysis* a logical plan for every rendered query in the project. This static analysis step is the cornerstone of Fusion's new capabilities.

| Step                                        | dbt Core engine | dbt Fusion engine |
| ------------------------------------------- | --------------- | ----------------- |
| Render Jinja into SQL                       | ✅              | ✅                |
| Produce and statically analyze logical plan | ❌              | ✅                |
| Run rendered SQL                            | ✅              | ✅                |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Principles of static analysis[​](#principles-of-static-analysis "Direct link to Principles of static analysis")

The concept of [static analysis](https://en.wikipedia.org/wiki/Static_program_analysis) is meant to guarantee that if a model compiles without error in development, it will also run without compilation errors when deployed. Introspective queries can break this promise by making it possible to modify the rendered query after a model is committed to source control.

The dbt Fusion engine uses the [`static_analysis`](https://docs.getdbt.com/reference/resource-configs/static-analysis.md) config to help you control how it performs static analysis for your models.

The dbt Fusion engine is unique in that it can statically analyze not just a single model in isolation, but every query from one end of your DAG to the other. Even your database can only validate the query in front of it! Concepts like [information flow theory](https://roundup.getdbt.com/i/156064124/beyond-cll-information-flow-theory-and-metadata-propagation) — although not incorporated into the dbt platform [yet](https://www.getdbt.com/blog/where-we-re-headed-with-the-dbt-fusion-engine) — rely on stable inputs and the ability to trace columns DAG-wide.

### Baseline mode: A smooth transition from dbt Core[​](#baseline-mode-a-smooth-transition-from-dbt-core "Direct link to Baseline mode: A smooth transition from dbt Core")

The dbt Fusion engine defaults to `static_analysis: baseline` mode, inspired by similar type-checking and linting tools like [TypeScript's migration approach](https://www.typescriptlang.org/docs/handbook/migrating-from-javascript.html), [basedpyright's baseline feature](https://docs.basedpyright.com/latest/benefits-over-pyright/baseline/), and [Pydantic's strict/lax modes](https://docs.pydantic.dev/latest/why/#strict-lax).

The philosophy behind the above-mentioned tools and Fusion's baseline mode is:

* **Smooth transition**: Provide a familiar first-time experience for users coming from dbt Core.
* **Incremental opt-in**: Offer a clear pathway to adopt more Fusion features over time.
* **Pragmatic validation**: Catch most SQL errors without requiring a complete project overhaul.

Use this style of gradual typing to start with lightweight validation, then incrementally adopt strict guarantees as your project is ready.

Migrating to Fusion can involve more than moving YAML around. Some scenarios that can make migration more involved include:

1. **Limited access to sources**: You don't have access to all the sources and models of a large dbt project.
2. **Intricate Jinja workflows**: Your project uses post-hooks and introspection extensively.
3. **Package compatibility**: Your project depends on packages that aren't yet Fusion-compatible.
4. **Unsupported SQL features**: Your models or sources use advanced data types (`STRUCT`, `ARRAY`, `GEOGRAPHY`) or built-in functions (`AI.PREDICT`, `JSON_FLATTEN`, `st_pointfromgeohash`) not yet supported by the dbt Fusion engine .

Setting `static_analysis` to `baseline` mode lets you start using Fusion immediately while you address these scenarios incrementally. As you resolve compatibility issues, you can opt specific models or your entire project into `strict` mode for maximum validation guarantees.

## Recapping the differences between engines[​](#recapping-the-differences-between-engines "Direct link to Recapping the differences between engines")

dbt Core:

* Renders and runs models one at a time.
* Never runs static analysis.

The dbt Fusion engine (baseline mode — default):

* Statically analyzes all models, catching most SQL errors while providing a familiar migration experience.

The dbt Fusion engine (strict mode):

* Renders and statically analyzes all models before execution begins.
* Guarantees nothing runs until the entire project is proven valid.

## Configuring `static_analysis`[​](#configuring-static_analysis "Direct link to configuring-static_analysis")

You can modify the way static analysis is applied for specific models in your project. Remember that **a model is only eligible for `strict` static analysis if all of its parents are also eligible.**

The [`static_analysis`](https://docs.getdbt.com/reference/resource-configs/static-analysis.md) config options are:

* `baseline` (default): Statically analyze SQL. This is the recommended starting point for users transitioning from dbt Core, providing a smooth migration experience while still catching most SQL errors.
* `strict` (previously `on`): Statically analyze all SQL before execution begins. Use this for maximum validation guarantees — nothing runs until the entire project is proven valid.
* `off`: Skip SQL analysis on this model and its descendants.

Deprecated values

The `on` and `unsafe` values are deprecated and will be removed in May 2026. Use `strict` instead.

When you disable static analysis, features of the VS Code extension which depend on SQL comprehension will be unavailable.

The best place to configure `static_analysis` is as a config on an individual model or group of models. As a debugging aid, you can also use the [`--static-analysis strict` or `--static-analysis off` CLI flags](https://docs.getdbt.com/reference/global-configs/static-analysis-flag.md) to override all model-level configuration.

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

This approach lets you gain the benefits of strict validation where possible while keeping the flexibility of baseline analysis for models that aren't yet compatible.

Refer to [CLI options](https://docs.getdbt.com/reference/global-configs/command-line-options.md) and [Configurations and properties](https://docs.getdbt.com/reference/configs-and-properties.md) to learn more about configs.

### Example configurations[​](#example-configurations "Direct link to Example configurations")

Disable static analysis for all models in a package:

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

Disable static analysis in YAML:

models/my\_udf\_using\_model.yml

```yml
models:
  - name: model_with_static_analysis_off
    config:
      static_analysis: off
```

Disable static analysis for a model using a custom UDF:

models/my\_udf\_using\_model.sql

```sql
{{ config(static_analysis='off') }}

select 
  user_id,
  my_cool_udf(ip_address) as cleaned_ip
from {{ ref('my_model') }}
```

### When should I turn static analysis `off`?[​](#when-should-i-turn-static-analysis-off "Direct link to when-should-i-turn-static-analysis-off")

Static analysis may incorrectly fail on valid queries if they contain:

* **syntax or native functions** that the dbt Fusion engine doesn't recognize. Please [open an issue](https://github.com/dbt-labs/dbt-fusion/issues) in addition to disabling static analysis.
* **dynamic SQL** such as [Snowflake's PIVOT ANY](https://docs.snowflake.com/en/sql-reference/constructs/pivot#dynamic-pivot-on-all-distinct-column-values-automatically) which cannot be statically analyzed. You can disable static analysis, refactor your pivot to use explicit column names, or create a [dynamic pivot in Jinja](https://github.com/dbt-labs/dbt-utils#pivot-source).
* **highly volatile data feeding an introspective query** during a standalone `dbt compile` invocation. Because the `dbt compile` step does not run models, it uses old data or defers to a different environment when running introspective queries. The more frequently the input data changes, the more likely it is for this divergence to cause a compilation error. Consider whether these standalone `dbt compile` commands are necessary before disabling static analysis.

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

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
