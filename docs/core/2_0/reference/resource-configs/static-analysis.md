# static\_analysis

static\_analysis controls how the Fusion engine analyzes SQL at compile time for models, tests, seeds, and snapshots.

info

The `static_analysis` config is available in the dbt Fusion engine only. It isn't available in dbt Core and will be ignored. To upgrade to Fusion, refer to [Get started with Fusion](https://docs.getdbt.com/docs/fusion/get-started-fusion.md).

The `static_analysis` config sets how the dbt Fusion engine validates SQL before execution—using `strict` analysis, a `baseline` that balances checks with compatibility, or `off` to skip analysis when needed. You can find supported configuration locations for each resource type.

To check out which Snowflake functions are supported in Fusion in `strict` mode, refer to [Snowflake function support](https://docs.getdbt.com/reference/resource-configs/snowflake-function-support.md). For BigQuery, refer to [BigQuery function support](https://docs.getdbt.com/reference/resource-configs/bigquery-function-support.md).

* Models
* Tests
* Seeds
* Snapshots

dbt\_project.yml

```yml
models:
  resource-path:
    +static_analysis: strict | baseline | off
```

models/filename.yml

```yml
models:
  - name: model_name
    config:
      static_analysis: strict | baseline | off
```

models/model\_name.sql

```sql
{{ config(static_analysis='strict' | 'baseline' | 'off') }}
```

dbt\_project.yml

```yml
data_tests:
  +static_analysis: strict | baseline | off
```

models/filename.yml

```yml
models:
  - name: model_name
    data_tests:
      - not_null:
          arguments:
            column_name: your_column_name
          config:
            static_analysis: strict | baseline | off
```

dbt\_project.yml

```yml
seeds:
  resource-path:
    +static_analysis: strict | baseline | off
```

seeds/filename.yml

```yml
seeds:
  - name: seed_name
    config:
      static_analysis: strict | baseline | off
```

dbt\_project.yml

```yml
snapshots:
  resource-path:
    +static_analysis: strict | baseline | off
```

snapshots/filename.yml

```yml
snapshots:
  - name: snapshot_name
    config:
      static_analysis: strict | baseline | off
```

## Definition[​](#definition "Direct link to Definition")

You can configure `static_analysis` for [models](https://docs.getdbt.com/docs/build/sql-models.md), [data tests](https://docs.getdbt.com/docs/build/data-tests.md), [seeds](https://docs.getdbt.com/docs/build/seeds.md), and [snapshots](https://docs.getdbt.com/docs/build/snapshots.md).

You can configure if and when the dbt Fusion engine performs static SQL analysis for a model. Configure the `static_analysis` config in your project YAML file (`dbt_project.yml`), model properties YAML file, or in a SQL config block in your model file. Refer to [Principles of static analysis](https://docs.getdbt.com/docs/fusion/new-concepts.md?version=1.12#principles-of-static-analysis) for more information on the different modes of static analysis.

Setting a model to `strict` does not automatically set `strict` for downstream models; they keep the project default unless you configure them explicitly. For more information and examples, refer to [strict mode inheritance](https://docs.getdbt.com/docs/fusion/new-concepts.md#strict-mode-inheritance).

The following values are available for `static_analysis`:

* `baseline` (default): Statically analyze SQL. This is the recommended starting point for users transitioning from dbt Core, providing a smooth migration experience while still catching most SQL errors. You can incrementally opt-in to stricter analysis over time.
* `strict` (previously `on`): Statically analyze all SQL before execution begins. Use this for maximum validation guarantees — nothing runs until the entire project is proven valid.
* `off`: Skip SQL analysis for this model and its descendants.

Deprecated values

The `on` and `unsafe` values are deprecated and will be removed in May 2026. Use `strict` instead.

### User-defined functions (UDFs) in `strict` mode[​](#user-defined-functions-udfs-in-strict-mode "Direct link to user-defined-functions-udfs-in-strict-mode")

When `static_analysis: strict` is in effect, the dbt Fusion engine parses `CREATE FUNCTION` statements from [`sql_header`](https://docs.getdbt.com/reference/resource-configs/sql_header.md) and from [`on-run-start`](https://docs.getdbt.com/reference/project-configs/on-run-start-on-run-end.md) project hooks, registers those UDFs in the compiler registry, and makes them available during strict static compilation. The `baseline` and `off` modes don't perform this UDF registration for static analysis.

A model’s `sql_header` can include multiple statements. Fusion registers UDFs from `CREATE FUNCTION` statements and ignores other statements for this step.

If strict analysis still cannot resolve a UDF, set [`static_analysis: off`](https://docs.getdbt.com/reference/resource-configs/static-analysis.md#disable-static-analysis-in-sql-for-a-model-using-a-custom-udf) on the affected models.

### How static analysis modes cascade[​](#how-static-analysis-modes-cascade "Direct link to How static analysis modes cascade")

Two rules determine how `static_analysis` modes apply in a lineage:

* Eligibility rule: A model is eligible for static analysis only if all of its "parents" are eligible (by parents, we mean the models that are upstream of the current model in the lineage).
* Strictness rule: A "child" model cannot be stricter than its parent (by child, we mean the models that are downstream of the current model in the lineage).

The static analysis configuration cascades from most strict to least strict. Here's the strictness hierarchy: `strict` → `baseline` → `off`

**Allowed downstream by parent mode**<br />When going downstream in your lineage, you can keep the same mode or relax it; but you cannot make a child stricter than its parent. The following table shows the allowed downstream modes by parent mode:

| Parent mode | Child can be                       |
| ----------- | ---------------------------------- |
| `strict`    | `strict`, `baseline`, or `off`     |
| `baseline`  | `baseline` or `off` (not `strict`) |
| `off`       | `off` only                         |

For example, for the lineage Model A → Model B → Model C:

* If Model A is `baseline`, you *cannot* set Model B to `strict`
* If Model A is `strict`, you *can* set Model B to `baseline`

This makes sure that stricter validation requirements don't apply downstream when parent models haven't met those requirements.

Refer to the Fusion concepts page for deeper discussion and visuals: [New concepts](https://docs.getdbt.com/docs/fusion/new-concepts.md). For more info on the JSON schema, refer to the [dbt-jsonschema file](https://github.com/dbt-labs/dbt-jsonschema/blob/1e2c1536fbdd421e49c8b65c51de619e3cd313ff/schemas/latest_fusion/dbt_project-latest-fusion.json#L4689).

## CLI override[​](#cli-override "Direct link to CLI override")

You can override model-level configuration for a run using the following CLI flags. For example, to disable static analysis for a run:

```bash
dbt run --static-analysis off # disable static analysis for all models
dbt run --static-analysis baseline # use baseline analysis for all models
```

See [static analysis CLI flag](https://docs.getdbt.com/reference/global-configs/static-analysis-flag.md).

## Examples[​](#examples "Direct link to Examples")

The following examples show how to disable or configure `static_analysis` for different scenarios:

* [Disable static analysis for all models in a package](#disable-static-analysis-for-all-models-in-a-package)
* [Disable static analysis in YAML for a single model](#disable-static-analysis-in-yaml-for-a-single-model)
* [Disable static analysis in SQL for a model using a custom UDF](#disable-static-analysis-in-sql-for-a-model-using-a-custom-udf)
* [Configure static analysis for tests](#configure-static-analysis-for-tests)
* [Configure static analysis for seeds](#configure-static-analysis-for-seeds)
* [Configure static analysis for snapshots](#configure-static-analysis-for-snapshots)

#### Disable static analysis for all models in a package[​](#disable-static-analysis-for-all-models-in-a-package "Direct link to Disable static analysis for all models in a package")

This example shows how to disable static analysis for all models in a package. The [`+` prefix](https://docs.getdbt.com/reference/resource-configs/plus-prefix.md) applies the config to all models in the package.

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

#### Disable static analysis in YAML for a single model[​](#disable-static-analysis-in-yaml-for-a-single-model "Direct link to Disable static analysis in YAML for a single model")

This example shows how to disable static analysis for a single model in YAML.

models/my\_udf\_using\_model.yml

```yml
models:
  - name: model_with_static_analysis_off
    config:
      static_analysis: off
```

#### Disable static analysis in SQL for a model using a custom UDF[​](#disable-static-analysis-in-sql-for-a-model-using-a-custom-udf "Direct link to Disable static analysis in SQL for a model using a custom UDF")

This example shows how to disable static analysis for a model using a custom [user-defined function (UDF)](https://docs.getdbt.com/docs/build/udfs.md) in a SQL file.

models/my\_udf\_using\_model.sql

```sql
{{ config(static_analysis='off') }}

select
  user_id,
  my_cool_udf(ip_address) as cleaned_ip
from {{ ref('my_model') }}
```

#### Configure static analysis for data tests[​](#configure-static-analysis-for-data-tests "Direct link to Configure static analysis for data tests")

This example shows how to set static analysis for all tests in a project using `dbt_project.yml`.

dbt\_project.yml

```yaml
# dbt_project.yml
data_tests:
  +static_analysis: baseline
```

To configure static analysis for a specific data test on a model:

models/filename.yml

```yaml
# models/filename.yml
models:
  - name: my_model
    data_tests:
      - not_null:
          arguments:
            column_name: order_id
          config:
            static_analysis: off
```

#### Configure static analysis for seeds[​](#configure-static-analysis-for-seeds "Direct link to Configure static analysis for seeds")

This example shows how to set static analysis for all seeds in a project.

dbt\_project.yml

```yaml
# dbt_project.yml
seeds:
  your_project:
    +static_analysis: baseline
```

To configure a single seed in a properties file:

seeds/filename.yml

```yaml
# seeds/filename.yml
seeds:
  - name: my_seed
    config:
      static_analysis: off
```

#### Configure static analysis for snapshots[​](#configure-static-analysis-for-snapshots "Direct link to Configure static analysis for snapshots")

This example shows how to set static analysis for all snapshots in a project.

dbt\_project.yml

```yaml
# dbt_project.yml
snapshots:
  your_project:
    +static_analysis: baseline
```

To configure a single snapshot in a properties file:

snapshots/filename.yml

```yaml
# snapshots/filename.yml
snapshots:
  - name: my_snapshot
    config:
      static_analysis: off
```

## Considerations[​](#considerations "Direct link to Considerations")

* For models, disabling static analysis means that features of the VS Code extension that depend on SQL comprehension will be unavailable.
* For models, static analysis can fail in some cases (for example, dynamic SQL constructs or unrecognized UDFs) and you might need to set `static_analysis: off`. For more examples, refer to [When should I turn static analysis off?](https://docs.getdbt.com/docs/fusion/new-concepts.md#when-should-i-turn-static-analysis-off).
