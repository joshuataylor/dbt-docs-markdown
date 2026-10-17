# Supported data platforms

dbt connects to and runs SQL against your database, warehouse, lake, or query engine. These SQL-speaking platforms are collectively referred to as *data platforms*. dbt connects with data platforms by using a dedicated adapter plugin for each. Plugins are built as Python modules that dbt v1 discovers if they are installed on your system. Refer to the [Build, test, document, and promote adapters](../guides/adapter-creation.md) guide for details.

Adapters are maintained by dbt Labs, partners, and community members. Refer to [community adapters](./community-adapters.md) for the combined list.

### Install an adapter

To use dbt from the command line, install the adapter for your data platform and set up a `profiles.yml` file. Refer to [installing dbt](./local/install-dbt.md).

(Applies to dbt v1.99 and earlier)

With a few exceptions [1](#user-content-fn-1), you can install adapters from PyPI with `python -m pip install adapter-name`. To install the Snowflake adapter, use `python -m pip install dbt-snowflake`. The installation includes dbt v1 and any other required dependencies.

| Adapter repo name | PyPI package name    |
| ----------------- | -------------------- |
| `dbt-layer`       | `dbt-layer-bigquery` |

Considerations for depending on an open-source project

1. Does it work?
2. Does anyone "own" the code, or is anyone liable for ensuring it works?
3. Do bugs get fixed quickly?
4. Does it stay up-to-date with new dbt v1 features?
5. Is the usage substantial enough to self-sustain?
6. Do other known projects depend on this library?

## Footnotes

1. Use the PyPI package name when installing with `pip`. [↩](#user-content-fnref-1)
