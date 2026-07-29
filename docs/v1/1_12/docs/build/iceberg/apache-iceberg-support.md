# Apache Iceberg support

Apache Iceberg is an open table format that brings greater portability and interoperability to the data ecosystem. By standardizing how data is stored and accessed, Iceberg enables teams to use the same data across multiple engines and platforms, without replication.

There are multiple layers of Iceberg support, in data platform and in dbt:

* **Iceberg table format** — An open-source table format. Iceberg tables are a combination of data files in object storage (such as Parquet files in an S3 bucket), as well as metadata files (recording the table's schema, versioning, and more) that also live in object storage.
* **Iceberg data catalog** — An open-source specification for a metadata system that tracks the schema, partition, and versions of multiple Iceberg tables.
* **Iceberg REST protocol** (also referred to as the Iceberg REST API) — Defines standard endpoints for interacting with Iceberg-compatible catalogs.

In theory, the Iceberg standard enables one query engine to read from or write to Iceberg tables in an external catalog, managed by another engine or platform. In practice, different Iceberg catalogs work differently, and different query engines support the Iceberg spec to varying degrees.

To the extent possible, dbt tries to abstract away the complexity of table formats, and the divergence among vendor-specific Iceberg implementations, so teams can focus on delivering reliable, well-modeled data. To learn more, select one of the following tiles:

[![](/img/icons/dbt-icon.svg)](https://docs.getdbt.com/docs/build/iceberg/about-catalogs.md)

#### [Iceberg catalogs](https://docs.getdbt.com/docs/build/iceberg/about-catalogs.md)

[About Iceberg catalogs](https://docs.getdbt.com/docs/build/iceberg/about-catalogs.md)

[![](/img/icons/dbt-icon.svg)](https://docs.getdbt.com/docs/build/iceberg/catalogs-yml.md)

#### [Using table\_format + catalogs.yml](https://docs.getdbt.com/docs/build/iceberg/catalogs-yml.md)

[dbt support for Iceberg + catalogs](https://docs.getdbt.com/docs/build/iceberg/catalogs-yml.md)

[![](/img/icons/snowflake.svg)](https://docs.getdbt.com/docs/build/iceberg/adapters/snowflake-iceberg-support.md)

#### [Snowflake + Iceberg](https://docs.getdbt.com/docs/build/iceberg/adapters/snowflake-iceberg-support.md)

[Snowflake Iceberg configurations](https://docs.getdbt.com/docs/build/iceberg/adapters/snowflake-iceberg-support.md)

[![](/img/icons/bigquery.svg)](https://docs.getdbt.com/docs/build/iceberg/adapters/bigquery-iceberg-support.md)

#### [BigQuery + Iceberg](https://docs.getdbt.com/docs/build/iceberg/adapters/bigquery-iceberg-support.md)

[BigQuery Iceberg configurations](https://docs.getdbt.com/docs/build/iceberg/adapters/bigquery-iceberg-support.md)

[![](/img/icons/databricks.svg)](https://docs.getdbt.com/docs/build/iceberg/adapters/databricks-iceberg-support.md)

#### [Databricks + Iceberg](https://docs.getdbt.com/docs/build/iceberg/adapters/databricks-iceberg-support.md)

[Databricks Iceberg configurations](https://docs.getdbt.com/docs/build/iceberg/adapters/databricks-iceberg-support.md)

[![](/img/icons/duckdb-seeklogo.svg)](https://docs.getdbt.com/docs/build/iceberg/adapters/duckdb-iceberg-support.md)

#### [DuckDB + Iceberg](https://docs.getdbt.com/docs/build/iceberg/adapters/duckdb-iceberg-support.md)

[DuckDB Iceberg configurations](https://docs.getdbt.com/docs/build/iceberg/adapters/duckdb-iceberg-support.md)
