# Databricks and Apache Iceberg

dbt supports materializing Iceberg tables in Unity Catalog using the catalog integration, starting with the dbt-databricks 1.9.0 release, for two Databricks materializations:

* [Table](https://docs.getdbt.com/docs/build/materializations.md#table)
* [Incremental](https://docs.getdbt.com/docs/build/materializations.md#incremental)

## Databricks Iceberg tables[​](#databricks-iceberg-tables "Direct link to Databricks Iceberg tables")

Databricks is built on [Delta Lake](https://docs.databricks.com/aws/en/delta/) and stores data in the [Delta table](https://docs.databricks.com/aws/en/introduction/delta-comparison#delta-tables-default-data-table-architecture) format.

Databricks supports two methods for creating Iceberg tables in its data catalog, [Unity Catalog](https://docs.databricks.com/aws/en/data-governance/unity-catalog/):

* Creating [Unity Catalog managed Iceberg tables](https://docs.databricks.com/aws/en/tables/managed). Databricks Runtime 16.4 LTS and later support this feature.
* Enabling [Iceberg reads](https://docs.databricks.com/aws/en/delta/uniform) on Delta tables. These tables still use the Delta file format, but generate both Delta and Iceberg-compatible metadata. Databricks Runtime 14.3 LTS and later support this feature.

External Iceberg compute engines can read from and write to these Iceberg tables using Unity Catalog's [Iceberg REST API endpoint](https://docs.databricks.com/aws/en/external-access/iceberg). However, Databricks only supports reading from external Iceberg catalogs.

To set up Databricks for reading and querying external tables, configure [Lakehouse Federation](https://docs.databricks.com/aws/en/query-federation/) and establish the catalog as a foreign catalog. Configure this outside of dbt. Once completed, it becomes another database you can query.

dbt does not yet support enabling [Iceberg v3](https://docs.databricks.com/aws/en/iceberg/iceberg-v3) on managed Iceberg tables.

## Creating Iceberg tables[​](#creating-iceberg-tables "Direct link to Creating Iceberg tables")

To configure dbt models to materialize as Iceberg tables, you can use a catalog integration with `table_format: iceberg` (see [dbt Catalog integration configurations for databricks](#dbt-catalog-integration-configurations-for-databricks)).

<!-- -->

<!-- -->

### External tables[​](#external-tables "Direct link to External tables")

dbt also supports creating externally-managed Iceberg tables using the model configuration [`location_root`](https://docs.getdbt.com/reference/resource-configs/databricks-configs.md#configuring-tables). Databricks' DDL for creating tables requires a fully qualified `location`. dbt defines this parameter on the user's behalf to streamline usage and enforce basic isolation of table data:

* When you set a `location_root` string, dbt generates a `location` string of the form: `{{ location_root }}/{{ model_name }}`. If you set the configuration option `include_full_name_in_path` to true, dbt generates a `location` string of the form `{{ location_root }}/{{ database_name}}/{{ schema_name }}/{{ model_name }}`.

<!-- -->

### dbt Catalog integration configurations for Databricks[​](#dbt-catalog-integration-configurations-for-databricks "Direct link to dbt Catalog integration configurations for Databricks")

<!-- -->

<!-- -->

#### Note[​](#note "Direct link to Note")

On Databricks, if a model has `catalog_name=<>` in its model config, the catalog name becomes the catalog part of the model's FQN. For example, if the catalog is named `my_database`, a model with `catalog_name='my_database'` is materialized as `my_database.<schema>.<model>`.

<!-- -->

## Configure catalog integration for Iceberg tables[​](#configure-catalog-integration-for-iceberg-tables "Direct link to Configure catalog integration for Iceberg tables")

1. Create a `catalogs.yml` at the top level of your dbt project (at the same level as dbt\_project.yml)
   <br />
   <br />
   An example of Unity Catalog as the catalog:

```yaml

catalogs:
  - name: unity_catalog
    active_write_integration: unity_catalog_integration
    write_integrations:
      - name: unity_catalog_integration
        table_format: iceberg
        catalog_type: unity
        file_format: delta   
```

2. Add the `catalog_name` config parameter in either a config block (inside the .sql model file), properties YAML file (model folder), or your project YAML file (`dbt_project.yml`).
   <br />

<br />

An example of `iceberg_model.sql`:

```yaml

{{
    config(
        materialized = 'table',
        catalog_name = 'unity_catalog'

    )
}}

select * from {{ ref('jaffle_shop_customers') }}
```

3. Execute the dbt model with a `dbt run -s iceberg_model`.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
