# Mapping project folders to warehouse schemas

Folders organize your code. Schemas (or datasets, in BigQuery) organize the tables and views that code builds in your warehouse. You can map folders to schemas so staging, intermediate, and marts are easy to find. dbt prefixes each schema with your target schema, so development doesn’t overwrite production.

## Map folders to custom schemas

By default, dbt builds every model in your target schema, which comes from the [environment](../../docs/dbt-platform-environments.md) in dbt platform or from the [profile](../../docs/local/dbt-environments.md) when you run locally (for example, development, CI, or production). To give the models in a folder their own schema, set `+schema` on that folder. dbt calls this a custom schema, which doesn't replace the target schema. Instead it joins the two as `<target_schema>_<custom_schema>`, so development and production build into different schemas. Refer to [Custom schemas](../../docs/build/custom-schemas.md).

We recommend naming your custom schemas after your project folders, so staging, intermediate, and marts models are easy to find in your warehouse. For example, set `+schema: staging`, `+schema: intermediate`, and `+schema: marts`. If your team already uses different names, use those instead.

For example, using `analytics_prod` and `alice_dev` as target schemas, dbt builds each folder into these schemas:

| Folder                 | Config                  | Production (`analytics_prod`) | Development (`alice_dev`) |
| ---------------------- | ----------------------- | ----------------------------- | ------------------------- |
| `models/staging/`      | `+schema: staging`      | `analytics_prod_staging`      | `alice_dev_staging`       |
| `models/intermediate/` | `+schema: intermediate` | `analytics_prod_intermediate` | `alice_dev_intermediate`  |
| `models/marts/`        | `+schema: marts`        | `analytics_prod_marts`        | `alice_dev_marts`         |

Sources point at tables that already exist, typically in `raw`. dbt reads those tables; it does not build them. For more information, refer to [Staging](./2-staging.md) and [Sources](../../docs/build/sources.md).

note

The [Jaffle Shop](https://github.com/dbt-labs/jaffle-shop) example can load seed data into `raw` when `load_source_data` is enabled. Those seeds use `+schema: raw` and land in the `raw` schema.

## Set custom schemas in `dbt_project.yml`

When you set `+schema` on the folder in `dbt_project.yml`, every model in that folder inherits it. Configure a schema on an individual model only when that model should use a different schema.

dbt\_project.yml

```yaml
models:
  jaffle_shop:
    staging:
      +materialized: view
      +schema: staging
    intermediate:
      +materialized: view
      +schema: intermediate
    marts:
      +materialized: table
      +schema: marts
```

The `jaffle_shop` key must match the `name` in your `dbt_project.yml`. The [Jaffle Shop](https://github.com/dbt-labs/jaffle-shop) example has `staging` and `marts` only; the `intermediate` block is unused until you add that folder.

This guide sets defaults on folders instead of on every model. Refer to [The rest of the project](./5-the-rest-of-the-project.md).

## Use only the custom schema in production

To use `marts` instead of `analytics_prod_marts` in production, use the built-in [`generate_schema_name_for_env`](../../docs/build/custom-schemas.md#a-built-in-alternative-pattern-for-generating-schema-names) pattern. It uses the custom schema alone when `target.name` is `prod`. In every other environment, it ignores custom schemas and builds all models in your target schema (`alice_dev`). Don't change the default macro to drop the target schema everywhere, or development runs can write to the same schemas as production.

## Use bronze, silver, and gold schema names

If your warehouse uses bronze, silver, and gold schema names, those names map to the same folders previously discussed. You don't need a different dbt layout.

The following table maps those names to the folders in this guide:

| Warehouse name | In this guide                |
| -------------- | ---------------------------- |
| Bronze         | Sources in `raw` (read-only) |
| Silver         | Staging and intermediate     |
| Gold           | Marts                        |

Keep `staging`, `intermediate`, and `marts` as the custom schema names in this guide so they match the folders. If your warehouse requires `silver` and `gold` as schema names, set `+schema` to those names instead.

If you set `+schema: silver` on both `staging` and `intermediate`, those models share one schema and you lose a separate intermediate schema. To keep that separation, use two custom schema names.

## Related docs

* [How we structure our dbt projects](./1-guide-overview.md)
* [Custom schemas](../../docs/build/custom-schemas.md)
* [Staging](./2-staging.md)
* [Intermediate](./3-intermediate.md)
* [Marts](./4-marts.md)
* [The rest of the project](./5-the-rest-of-the-project.md)
* [Optimize dbt models on Databricks](../../guides/optimize-dbt-models-on-databricks.md)
* [Best practices for dbt and Unity Catalog](../dbt-unity-catalog-best-practices.md)
