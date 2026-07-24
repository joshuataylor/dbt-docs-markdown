# Fusion availability

Use the following tables to see where Fusion is available for your adapter. You can get started with many dbt features right away, and some more are available when you sign in with any dbt platform account, free or paid.

## Adapter lifecycle[​](#adapter-lifecycle "Direct link to Adapter lifecycle")

Fusion is available across adapters (data warehouse connectors). Track status by adapter using the following table:

| Adapter                 | Lifecycle |
| ----------------------- | --------- |
| Snowflake               | Preview   |
| BigQuery                | Preview   |
| Databricks              | Preview   |
| Redshift                | Preview   |
| Apache Spark (CLI only) | Beta      |
| DuckDB (CLI only)       | Beta      |

*Note that adapter lifecycle may differ between the dbt platform and local development. An adapter can reach GA in the dbt platform before it reaches GA for local use.*

## What you get with Fusion[​](#what-you-get-with-fusion "Direct link to What you get with Fusion")

Feature availability

Feature availability may change as the Fusion engine moves toward general availability.

v2 is totally free to use, including when paired with Fusion, and you can get started right away with many dbt features, free forever! You can also try advanced features by running [`dbt login`](https://docs.getdbt.com/reference/commands/login.md) to create a free dbt platform account for the best v2 experience:

```shell
dbt login
```

Creating an account also unlocks additional free-tier access to dbt services.

| Feature                                               | Free forever (for real!) | Requires login<br />to any dbt platform account, free or paid |
| ----------------------------------------------------- | ------------------------ | ------------------------------------------------------------- |
| dbt Core v1.x workflows, except dbt docs v1           | ✅                       | ✅                                                            |
| Syntax error detection (Jinja, YAML, SQL)             | ✅                       | ✅                                                            |
| dbt lint                                              | ✅                       | ✅                                                            |
| dbt docs v2 (lite)                                    | ✅                       | ✅                                                            |
| LSP (lite): go-to ref, source, and macro              | ✅                       | ✅                                                            |
| Full LSP: CTE, hover to see schema, and more          | -                        | ✅                                                            |
| SQL comprehension, type checking, and impact analysis | -                        | ✅                                                            |
| Precise column-level lineage                          | -                        | ✅                                                            |
| dbt docs v2 (full), including column-level lineage    | -                        | ✅                                                            |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

For the best v2 experience, use the dbt VS Code extension. You can get started for free, and when you create a free dbt platform account, you’ll unlock additional access to advanced dbt features in your editor and beyond, including those shown in the table above.

To learn more about VS Code-specific capabilities, refer to [dbt VS Code extension features](https://docs.getdbt.com/docs/dbt-extension-features.md).

## Related docs[​](#related-docs "Direct link to Related docs")

* [Install dbt](https://docs.getdbt.com/docs/local/install-dbt.md) locally
* Install the [dbt VS Code extension](https://docs.getdbt.com/docs/about-dbt-extension.md)
* Upgrade environments in the [dbt platform](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md#dbt-fusion-engine)
