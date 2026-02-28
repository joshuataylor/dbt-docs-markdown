# BigQuery and Apache Iceberg

dbt supports materializing Iceberg tables on BigQuery via the catalog integration, starting with the dbt-bigquery 1.10 release.

## Creating Iceberg Tables[​](#creating-iceberg-tables "Direct link to Creating Iceberg Tables")

dbt supports creating Iceberg tables for two of the BigQuery materializations:

* [Table](https://docs.getdbt.com/docs/build/materializations.md#table)
* [Incremental](https://docs.getdbt.com/docs/build/materializations.md#incremental)

## BigQuery Iceberg catalogs[​](#bigquery-iceberg-catalogs "Direct link to BigQuery Iceberg catalogs")

BigQuery supports Iceberg tables via its built-in catalog [BigLake Metastore](https://cloud.google.com/bigquery/docs/iceberg-tables#architecture) today. No setup is needed to access the BigLake Metastore. However, you will need to have a [storage bucket](https://docs.cloud.google.com/storage/docs/buckets#buckets) and [the required BigQuery roles](https://cloud.google.com/bigquery/docs/iceberg-tables#required-roles) configured prior to creating an Iceberg table.

### dbt Catalog integration configurations for BigQuery[​](#dbt-catalog-integration-configurations-for-bigquery "Direct link to dbt Catalog integration configurations for BigQuery")

The following table outlines the configuration fields required to set up a catalog integration for [Biglake Iceberg tables in BigQuery](https://docs.cloud.google.com/bigquery/docs/iceberg-tables).

<!-- -->

<!-- -->

### Configure catalog integration for managed Iceberg tables[​](#configure-catalog-integration-for-managed-iceberg-tables "Direct link to Configure catalog integration for managed Iceberg tables")

1. Create a `catalogs.yml` at the top level of your dbt project.
   <br />
   <br />
   An example:

```yaml

catalogs:
  - name: my_bigquery_iceberg_catalog
    active_write_integration: biglake_metastore
    write_integrations:
      - name: biglake_metastore
        external_volume: 'gs://mydbtbucket'
        table_format: iceberg
        file_format: parquet
        catalog_type: biglake_metastore
```

2. Apply the catalog configuration at either the model, folder, or project level:

iceberg\_model.sql

```sql

{{
    config(
        materialized='table',
        catalog_name='my_bigquery_iceberg_catalog'

    )
}}

select * from {{ ref('jaffle_shop_customers') }}
```

3. Execute the dbt model with `dbt run -s iceberg_model`.

### Limitations[​](#limitations "Direct link to Limitations")

BigQuery today does not support connecting to external Iceberg catalogs. In terms of SQL operations and table management features, please refer to the [BigQuery docs](https://cloud.google.com/bigquery/docs/iceberg-tables#limitations) for more information.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
