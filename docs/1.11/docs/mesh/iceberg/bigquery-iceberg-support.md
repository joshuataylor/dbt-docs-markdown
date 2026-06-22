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

| Field             | Required | Accepted values                                                                    |
| ----------------- | -------- | ---------------------------------------------------------------------------------- |
| `name`            | yes      | Name of catalog integration                                                        |
| `catalog_name`    | yes      | The name of the catalog integration in BigQuery. For example, `biglake_metastore`. |
| `external_volume` | yes      | `gs://<bucket_name>`                                                               |
| `table_format`    | yes      | `iceberg`                                                                          |
| `catalog_type`    | yes      | `biglake_metastore`                                                                |
| `file_format`     | yes      | `parquet`                                                                          |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

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

### Base location[​](#base-location "Direct link to Base location")

BigQuery's DDL for creating iceberg tables requires that a fully qualified storage\_uri be provided, including the object path. Once the user has provided the bucket name as the `external_volume` in the catalog integration, dbt will manage the storage\_uri input. The default behavior in dbt is to provide an object path, referred to in dbt as the `base_location`, in the form: `_dbt/{SCHEMA_NAME}/{MODEL_NAME}`. We recommend using the default behavior, but if you need to customize the resulting `base_location`, dbt allows users to configure `base_location` with the model configuration fields `base_location_root` and `base_location_subpath`.

* If no inputs are provided, dbt will output for base\_location `{{ external_volume }}/_dbt/{{ schema }}/{{ model_name }}`
* If base\_location\_root = `foo`, dbt will output `{{ external_volume }}/foo/{{ schema }}/{{ model_name }}`
* If base\_location\_subpath = `bar`, dbt will output `{{ external_volume }}/_dbt/{{ schema }}/{{ model_name }}/bar`
* If base\_location\_root = `foo` and base\_location\_subpath = `bar`, dbt will output `{{ external_volume }}/foo/{{ schema }}/{{ model_name }}/bar`

note

While you can customize paths with `base_location_root` and `base_location_subpath`, we don't recommend relying on them for environment isolation (such as separating development and production environments). Anyone with repository access can easily modify these configuration values. For true environment isolation, use separate `external_volume` values with infrastructure-level access controls.

dbt also allows users to completely override the storage\_uri with the model configuration field `storage_uri`. This overrides both the catalog integration path and the other model configuration fields to supply the entire `storage_uri` path directly.

An example model with a customized `base_location`:

iceberg\_model.sql

```sql

{{
    config(
        materialized='table',
        catalog_name='my_bigquery_iceberg_catalog',
        base_location_root='foo',
        base_location_subpath='bar',

    )
}}

select * from {{ ref('jaffle_shop_customers') }}
```

#### Rationale[​](#rationale "Direct link to Rationale")

By default, dbt manages the full `storage_uri` on behalf of users for ease of use. The `base_location` parameter specifies the location within the storage bucket where the data will be written. Without guardrails (for example, if the user forgets to provide a base location root), it's possible for BigQuery to reuse the same path across multiple tables.

This behavior could result in future technical debt because it will limit the ability to:

* Navigate the underlying object store
* Read Iceberg tables via an object-store integration
* Grant schema-specific access to tables via object store
* Use a crawler pointed at the tables within the external storage to build a new catalog with another tool

To maintain best practices, dbt enforces an input and, by default, writes your tables within a `_dbt/{SCHEMA_NAME}/{TABLE_NAME}` prefix to ensure easier object-store observability and auditability.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
