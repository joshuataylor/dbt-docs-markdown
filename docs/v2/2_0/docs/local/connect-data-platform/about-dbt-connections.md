# About data platform connections

Local development

dbt connects to your data platform to run SQL transformations against your data.

(Applies to dbt v2.0 and later)

## Supported Fusion data platforms

The dbt Fusion engine includes built-in support for:

* [Snowflake](./snowflake-setup.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [Databricks](./databricks-setup.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [Amazon Redshift](./redshift-setup.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [Google BigQuery](./bigquery-setup.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [DuckDB](./duckdb-setup.md) [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [Apache Spark](./spark-setup.md) [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

*Adapter lifecycle can differ between the dbt platform and local development. An adapter can reach GA in the dbt platform before it reaches GA for local use.*

Fusion uses [ADBC (Arrow Database Connectivity)](https://arrow.apache.org/adbc/) drivers for high-performance connections to these platforms. No separate adapter installation is required.

## Connection profiles

When you run dbt locally, it reads your `dbt_project.yml` file to find the profile name, then looks for a profile with the same name in your `profiles.yml` file. This profile contains the connection details for your data platform.

For detailed configuration options, refer to [Connection profiles](../profiles.yml.md).

## Next steps

For step-by-step setup instructions with demo project data, see our [Quickstart guides](https://docs.getdbt.com/guides).
