# static\_analysis

info

The `static_analysis` config is available in the dbt Fusion engine only. It isn't available in dbt Core and will be ignored. To upgrade to Fusion, refer to [Get started with Fusion](https://docs.getdbt.com/docs/fusion/get-started-fusion.md).

* Project YAML file
* Properties YAML file
* SQL file config

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

select 
  user_id,
  my_cool_udf(ip_address) as cleaned_ip
from {{ ref('my_model') }}
```

## Definition[​](#definition "Direct link to Definition")

You can configure if and when the dbt Fusion engine performs static SQL analysis for a model. Configure the `static_analysis` config in your project YAML file (`dbt_project.yml`), model properties YAML file, or in a SQL config block in your model file. Refer to [rendering strategies](https://docs.getdbt.com/docs/fusion/new-concepts.md#rendering-strategies) for more information on how the dbt Fusion engine renders models.

The following values are available for `static_analysis`:

* `baseline` (default): Statically analyze SQL. This is the recommended starting point for users transitioning from dbt Core, providing a smooth migration experience while still catching most SQL errors. You can incrementally opt-in to stricter analysis over time.
* `strict` (previously `on`): Statically analyze all SQL before execution begins. Use this for maximum validation guarantees — nothing runs until the entire project is proven valid.
* `off`: Skip SQL analysis for this model and its descendants.

Deprecated values

The `on` and `unsafe` values are deprecated and will be removed in May 2026. Use `strict` instead.

A model is *only* eligible for static analysis if all of its parents are also eligible.

Refer to the Fusion concepts page for deeper discussion and visuals: [New concepts](https://docs.getdbt.com/docs/fusion/new-concepts.md). For more info on the JSON schema, refer to the [dbt-jsonschema file](https://github.com/dbt-labs/dbt-jsonschema/blob/1e2c1536fbdd421e49c8b65c51de619e3cd313ff/schemas/latest_fusion/dbt_project-latest-fusion.json#L4689).

## CLI override[​](#cli-override "Direct link to CLI override")

You can override model-level configuration for a run using the following CLI flags. For example, to disable static analysis for a run:

```bash
dbt run --static-analysis off # disable static analysis for all models
dbt run --static-analysis baseline # use baseline analysis for all models
```

See [static analysis CLI flag](https://docs.getdbt.com/reference/global-configs/static-analysis-flag.md).

## Examples[​](#examples "Direct link to Examples")

The following examples show how to disable static analysis for all models in a package, for a single model, and for a model that uses a custom UDF.

* [Disable static analysis for all models in a package](#disable-static-analysis-for-all-models-in-a-package)
* [Disable static analysis in YAML for a single model](#disable-static-analysis-in-yaml-for-a-single-model)
* [Disable static analysis in SQL for a model using a custom UDF](#disable-static-analysis-in-sql-for-a-model-using-a-custom-udf)

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

## Considerations[​](#considerations "Direct link to Considerations")

* Disabling static analysis means that features of the VS Code extension that depend on SQL comprehension will be unavailable.
* Static analysis might fail in some cases (for example, dynamic SQL constructs or unrecognized UDFs) and may require setting `static_analysis: off`. For more examples, refer to [When should I turn static analysis off?](https://docs.getdbt.com/docs/fusion/new-concepts.md#when-should-i-turn-static-analysis-off).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
