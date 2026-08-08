# About data platform connections

Local developmentⓘ

dbt connects to your data platform to run SQL transformations against your data.

<!-- -->

## Supported Core data platforms[​](#supported-core-data-platforms "Direct link to Supported Core data platforms")

dbt Core connects to data platforms through adapters. Popular platforms include:

* [Snowflake](./snowflake-setup.md)
* [Databricks](./databricks-setup.md)
* [Amazon Redshift](./redshift-setup.md)
* [Google BigQuery](./bigquery-setup.md)
* [PostgreSQL](./postgres-setup.md)
* [Apache Spark](./spark-setup.md)
* [Starburst or Trino](./trino-setup.md)
* [Microsoft Fabric](./fabric-setup.md)
* [Azure Synapse](./azuresynapse-setup.md)

When you install dbt Core, you also need to install the specific adapter for your data platform. Data platform adapters may be verified by our [Trusted Adapter Program](../../trusted-adapters.md), and maintained by dbt Labs, partners, or community members.

For the full list of supported platforms, see [Supported data platforms](../../supported-data-platforms.md).

## Connection profiles[​](#connection-profiles-1 "Direct link to Connection profiles")

When you run dbt from the CLI, it reads your `dbt_project.yml` file to find the profile name, then looks for a profile with the same name in your `profiles.yml` file. This profile contains the connection details for your data platform.

For detailed configuration options, refer to [Connection profiles](../profiles.yml.md).

## Adapter features[​](#adapter-features "Direct link to Adapter features")

The following table lists features available for adapters:

| Adapter                   | Catalog          | Source freshness                     |
| ------------------------- | ---------------- | ------------------------------------ |
| dbt default configuration | full             | `loaded_at_field`                    |
| `dbt-bigquery`            | partial and full | metadata-based and `loaded_at_field` |
| `dbt-databricks`          | full             | metadata-based and `loaded_at_field` |
| `dbt-postgres`            | partial and full | `loaded_at_field`                    |
| `dbt-redshift`            | partial and full | metadata-based and `loaded_at_field` |
| `dbt-snowflake`           | partial and full | metadata-based and `loaded_at_field` |
| `dbt-spark`               | full             | `loaded_at_field`                    |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Catalog[​](#catalog "Direct link to Catalog")

For adapters that support it, you can partially build the catalog (dbt Core v1.x only). This builds the catalog for only selected models via `dbt docs generate --select ...`. For adapters that don't support partial catalog generation, run `dbt docs generate` to build the full catalog.

### Source freshness[​](#source-freshness "Direct link to Source freshness")

You can measure source freshness using warehouse metadata tables on supported adapters. This calculates source freshness without using the [`loaded_at_field`](../../../reference/resource-properties/freshness.md#loaded_at_field) and without querying the table directly. This approach is faster and more flexible (though it might sometimes be inaccurate, depending on how the warehouse tracks altered tables). You can override this with the `loaded_at_field` in the [source config](../../../reference/source-configs.md). If the adapter doesn't support this, you can still use the `loaded_at_field`.

## Next steps[​](#next-steps "Direct link to Next steps")

For step-by-step setup instructions with demo project data, see our [Quickstart guides](../../../guides.md).
