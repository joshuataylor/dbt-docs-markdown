# Supported data platforms

dbt connects to and runs SQL against your database, warehouse, lake, or query engine. These SQL-speaking platforms are collectively referred to as *data platforms*. dbt connects with data platforms by using a dedicated adapter plugin for each. Plugins are built as Python modules that dbt v1 discovers if they are installed on your system. Refer to the [Build, test, document, and promote adapters](../guides/adapter-creation.md) guide for details.

Adapters are maintained by dbt Labs, partners, and community members. Refer to [community adapters](./community-adapters.md) for the combined list.

### Install an adapter

To use dbt from the command line, install the adapter for your data platform and set up a `profiles.yml` file. Refer to [installing dbt](./local/install-dbt.md).

(Applies to dbt v2.0 and later)

Adapters ship with dbt v2. When you [install dbt](./local/install-dbt.md), the supported data platforms are available with no separate `pip install` for each adapter.

Refer to [Contribute a dbt v2 adapter](../guides/adapter-creation-v2.md?step=1) for more information.

(Applies to dbt v2.0 and later)

## Adapter lifecycle

dbt v2 is available across adapters (data warehouse connectors). Track status by adapter using the following table:

| Adapter                 | Lifecycle           |
| ----------------------- | ------------------- |
| Snowflake               | Generally available |
| BigQuery                | Generally available |
| Databricks              | Generally available |
| Redshift                | Generally available |
| Apache Spark (CLI only) | Beta                |
| DuckDB (CLI only)       | Generally available |
| ClickHouse              | Private beta        |

*Note that adapter lifecycle may differ between the dbt platform and local development. An adapter can reach GA in the dbt platform before it reaches GA for local use.*

Considerations for depending on an open-source project

1. Does it work?
2. Does anyone "own" the code, or is anyone liable for ensuring it works?
3. Do bugs get fixed quickly?
4. Does it stay up-to-date with new dbt v1 features?
5. Is the usage substantial enough to self-sustain?
6. Do other known projects depend on this library?

## Footnotes

1. Use the PyPI package name when installing with `pip`. [↩](#user-content-fnref-1)
