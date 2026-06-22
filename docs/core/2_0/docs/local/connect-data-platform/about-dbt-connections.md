# About data platform connections

dbt connects to your data platform to run SQL transformations against your data. The connection setup depends on which dbt engine you use:

* [dbt Fusion engine](https://docs.getdbt.com/docs/local/connect-data-platform/about-dbt-connections.md?version=2)
* [dbt Core](https://docs.getdbt.com/docs/local/connect-data-platform/about-dbt-connections.md?version=1)

## Supported Fusion data platforms[​](#supported-fusion-data-platforms "Direct link to Supported Fusion data platforms")

The dbt Fusion engine includes built-in support for:

* [Snowflake](https://docs.getdbt.com/docs/local/connect-data-platform/snowflake-setup.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [Databricks](https://docs.getdbt.com/docs/local/connect-data-platform/databricks-setup.md) [Private preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [Amazon Redshift](https://docs.getdbt.com/docs/local/connect-data-platform/redshift-setup.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [Google BigQuery](https://docs.getdbt.com/docs/local/connect-data-platform/bigquery-setup.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [Salesforce Data 360](https://docs.getdbt.com/docs/local/connect-data-platform/salesforce-data-cloud-setup.md) [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [DuckDB](https://docs.getdbt.com/docs/local/connect-data-platform/duckdb-setup.md) [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [Apache Spark](https://docs.getdbt.com/docs/local/connect-data-platform/spark-setup.md) [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Fusion uses [ADBC (Arrow Database Connectivity)](https://arrow.apache.org/adbc/) drivers for high-performance connections to these platforms. No separate adapter installation is required.

## Connection profiles[​](#connection-profiles "Direct link to Connection profiles")

When you run dbt locally, it reads your `dbt_project.yml` file to find the profile name, then looks for a profile with the same name in your `profiles.yml` file. This profile contains the connection details for your data platform.

For detailed configuration options, refer to [Connection profiles](https://docs.getdbt.com/docs/local/profiles.yml.md).

<!-- -->

## Next steps[​](#next-steps "Direct link to Next steps")

For step-by-step setup instructions with demo project data, see our [Quickstart guides](https://docs.getdbt.com/guides.md).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
