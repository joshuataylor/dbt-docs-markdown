# Apache Iceberg Support

Apache Iceberg is an open standard table format that brings greater portability and interoperability to the data ecosystem. By standardizing how data is stored and accessed, Iceberg enables teams to work across different engines and platforms with confidence. It has many components to it but the main ones that dbt interacts with are:

* **Iceberg Table Format** - an open-source table format. Tables materialized in iceberg table format are stored on a user’s infrastructure, such as a S3 Bucket.
* **Iceberg Data Catalog** - an open-source metadata management system that tracks the schema, partition, and versions of Iceberg tables.
* **Iceberg REST Protocol** (also referred to as Iceberg REST API) is how engines can support and speak to other Iceberg-compatible catalogs.

dbt abstracts the complexity of table formats so teams can focus on delivering reliable, well-modeled data. Our initial integration with Iceberg supports table materializations and catalog integrations, allowing users to define and manage Iceberg tables directly in their dbt projects. To learn more, click on one of the following tiles

[![](/img/icons/dbt-icon.svg)](https://docs.getdbt.com/docs/mesh/iceberg/about-catalogs.md)

#### [Using dbt + Iceberg Catalog overview](https://docs.getdbt.com/docs/mesh/iceberg/about-catalogs.md)

[dbt support for Apache Iceberg](https://docs.getdbt.com/docs/mesh/iceberg/about-catalogs.md)

[![](/img/icons/snowflake.svg)](https://docs.getdbt.com/docs/mesh/iceberg/snowflake-iceberg-support.md)

#### [Snowflake](https://docs.getdbt.com/docs/mesh/iceberg/snowflake-iceberg-support.md)

[Snowflake Iceberg Configurations](https://docs.getdbt.com/docs/mesh/iceberg/snowflake-iceberg-support.md)

[![](/img/icons/bigquery.svg)](https://docs.getdbt.com/docs/mesh/iceberg/bigquery-iceberg-support.md)

#### [BigQuery](https://docs.getdbt.com/docs/mesh/iceberg/bigquery-iceberg-support.md)

[BigQuery Iceberg Configurations](https://docs.getdbt.com/docs/mesh/iceberg/bigquery-iceberg-support.md)

[![](/img/icons/databricks.svg)](https://docs.getdbt.com/docs/mesh/iceberg/databricks-iceberg-support.md)

#### [Databricks](https://docs.getdbt.com/docs/mesh/iceberg/databricks-iceberg-support.md)

[Databricks Iceberg Configurations](https://docs.getdbt.com/docs/mesh/iceberg/databricks-iceberg-support.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
