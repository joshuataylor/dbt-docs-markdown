# Fusion availability

The dbt Fusion engine and dbt Core v2 are currently in active development. Refer to the following tables to track lifecycle status and feature availability across the available products.

## Fusion lifecycle[​](#fusion-lifecycle "Direct link to Fusion lifecycle")

The dbt Fusion engine and dbt Core v2 are in various stages of development depending on deployment type (dbt platform or installed locally). Additionally, the available adapters (data warehouse connectors) are each in various states of their lifecycle. You can track both using the following table:

| Deployment                | Adapter                 | Status          |
| ------------------------- | ----------------------- | --------------- |
| **dbt Core v2.0 (local)** |                         | Alpha           |
| **Fusion (local)**        |                         | Preview         |
|                           | Snowflake               | Preview         |
|                           | BigQuery                | Preview         |
|                           | Databricks              | Private preview |
|                           | Redshift                | Preview         |
|                           | Apache Spark (CLI only) | Beta            |
|                           | DuckDB (CLI only)       | Beta            |
| **Fusion (dbt platform)** |                         | GA              |
|                           | Snowflake               | GA              |
|                           | BigQuery                | Preview         |
|                           | Databricks              | Private preview |
|                           | Redshift                | Preview         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Fusion and dbt Core v2 features[​](#fusion-and-dbt-core-v2-features "Direct link to Fusion and dbt Core v2 features")

Feature availability

Fusion is in preview, and dbt Core v2 is in alpha. As these products move toward general availability, features may evolve and registration requirements may change. For more information, refer to [Product lifecycles](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles.md).

dbt Core v2 and Fusion represent the next evolution of the classic dbt Core toolset, with performance enhancements and powerful new features. dbt Core v2 includes everything available in dbt Core v1.x, plus the speed and power of Fusion. Level up your development by [installing the dbt Fusion engine](https://docs.getdbt.com/docs/local/install-dbt.md?version=2).

To access the full set of features, register with your email address or sign in to your dbt platform account using `dbt login`. The following table shows which features are available to all users and which require registration:

| Feature                                                |  dbt Core v2<br />(OSS) | Fusion<br />(all users) | Fusion<br />(with registration) |
| ------------------------------------------------------ | ----------------------- | ----------------------- | ------------------------------- |
| Everything in dbt Core today<br />(except dbt docs v1) | ✅                      | ✅                      | ✅                              |
| dbt docs v2 (lite)                                     | ✅                      | ✅                      | ✅                              |
| Syntax error detection                                 | -                       | ✅                      | ✅                              |
| LSP (lite)                                             | -                       | ✅                      | ✅                              |
| dbt lint                                               | -                       | ✅                      | ✅                              |
| SQL Comprehension                                      | -                       | -                       | ✅                              |
| Full LSP                                               | -                       | -                       | ✅                              |
| Query cache                                            | -                       | -                       | ✅                              |
| dbt docs v2 (full)                                     | -                       | -                       | ✅                              |
| Auto-deferral                                          | -                       | -                       | ✅                              |
| Compare changes                                        | -                       | -                       | ✅                              |
| Precise column-level lineage artifact generation       | -                       | -                       | ✅                              |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## dbt VS Code extension features[​](#dbt-vs-code-extension-features "Direct link to dbt VS Code extension features")

The dbt VS Code extension gives you a powerful set of features to level up your development workflows. To access the full set, register with your email address or sign in with your dbt platform account. The following table shows which features are available to all users and which require registration:

| Feature                                                          | Available to all | Available to registered users |
| ---------------------------------------------------------------- | ---------------- | ----------------------------- |
| Error diagnostics for Jinja, YAML, and SQL syntax errors         | ✅               | ✅                            |
| Jinja LSP go-to ref definition                                   | ✅               | ✅                            |
| Jinja LSP go-to source definition                                | ✅               | ✅                            |
| Jinja LSP go-to macro definition                                 | ✅               | ✅                            |
| Diagnostics for linter warnings                                  | ✅               | ✅                            |
| Table-level lineage visualization                                | ✅               | ✅                            |
| Basic dbt command UI<br />(Run, Build, Test + Query Results Tab) | ✅               | ✅                            |
| Ref autocomplete                                                 | ✅               | ✅                            |
| Autorefactor ref names                                           | ✅               | ✅                            |
| Autorefactor column names on change                              | ✅               | ✅                            |
| Dialect-aware function autocomplete                              | ✅               | ✅                            |
| Error diagnostics for SQL types, schema errors, and more         | -                | ✅                            |
| Preview CTE                                                      | -                | ✅                            |
| Query cache for faster incremental compiles                      | -                | ✅                            |
| Model docs tab with platform metadata hydration                  | -                | ✅                            |
| Column-level lineage visualization                               | -                | ✅                            |
| Compare changes                                                  | -                | ✅                            |
| SQL LSP go-to Column definition                                  | -                | ✅                            |
| SQL LSP go-to CTE definition                                     | -                | ✅                            |
| SQL LSP hover to see schema for select \*                        | -                | ✅                            |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Develop with Fusion[​](#develop-with-fusion "Direct link to Develop with Fusion")

Not sure where to start?

Try out the [Fusion quickstart](https://docs.getdbt.com/guides/fusion.md) and check out the [Fusion migration guide](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md) to see how to migrate your project.

dbt Fusion engine powers dbt development everywhere, including the [dbt platform](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md#dbt-fusion-engine), [VS Code/Cursor/Windsurf](https://docs.getdbt.com/docs/about-dbt-extension.md), and [locally](https://docs.getdbt.com/docs/local/install-dbt.md).

Check out the following table to see what development solutions are available and where you can use them:

|                                                      | Features you can use                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | Who can use it?                                                                     | Solutions available                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| ---------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **dbt platform**<br />with Fusion or dbt Core engine | - [Canvas](https://docs.getdbt.com/docs/platform/canvas.md)<br />- [Insights](https://docs.getdbt.com/docs/explore/navigate-dbt-insights.md#lsp-features)<br />- [Studio IDE](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md)<br />- [dbt CLI](https://docs.getdbt.com/docs/platform/dbt-cli-installation.md)<br />- [dbt VS Code extension](https://marketplace.visualstudio.com/items?itemName=dbtLabsInc.dbt)<br />(VS Code/ Cursor/ Windsurf. Fusion only.) | - dbt platform licensed users<br />- Anyone getting started with dbt<br />          | - **dbt Fusion engine**: Rust-based engine that delivers fast, reliable compilation, analysis, validation, state awareness, and job execution with [visual LSP features](https://docs.getdbt.com/docs/fusion/supported-features.md#features-and-capabilities) like autocomplete, inline errors, live previews, and lineage, and more.<br /><br />- **dbt Core**: Uses the Python-based dbt Core engine for traditional workflows. *Does not* include LSP features. |
| **Self-hosted Fusion**                               | - [dbt VS Code extension](https://marketplace.visualstudio.com/items?itemName=dbtLabsInc.dbt)<br />(VS Code/Cursor/Windsurf)<br />- [Fusion CLI](https://docs.getdbt.com/docs/local/install-dbt.md?version=2)                                                                                                                                                                                                                                                                          | - dbt platform users<br />- dbt Fusion users<br />- Anyone getting started with dbt | - **VS Code extension:** Combines dbt Fusion engine performance with visual LSP features when developing locally.<br /><br />- **Fusion CLI:** Provides Fusion performance benefits (faster parsing, compilation, execution) but *does not* include LSP features.                                                                                                                                                                                                  |
| **Self-hosted dbt Core v1**                          | - [dbt Core v1 CLI](https://docs.getdbt.com/docs/local/install-dbt.md)                                                                                                                                                                                                                                                                                                                                                                                                                 | - dbt Core users<br />- Anyone getting started with dbt                             | Uses the Python-based dbt Core engine for traditional workflows. *Does not* include LSP features.<br /><br />To use the Fusion features locally, install [the VS Code extension](https://docs.getdbt.com/docs/local/install-dbt.md?version=2) or [Fusion CLI](https://docs.getdbt.com/docs/local/install-dbt.md?version=2).                                                                                                                                        |
| **Self-hosted dbt Core v2**                          | **Coming soon**                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | - Currently in Alpha                                                                | Uses the Rust-based dbt Fusion engine.                                                                                                                                                                                                                                                                                                                                                                                                                             |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Additional resources[​](#additional-resources "Direct link to Additional resources")

* Install Fusion locally from the [CLI](https://docs.getdbt.com/docs/local/install-dbt.md?version=2)
* Install the [dbt VS Code extension](https://docs.getdbt.com/docs/local/install-dbt.md?version=2)
* Upgrade environments to the [dbt Fusion engine](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md#dbt-fusion-engine)
* Fusion [features](https://docs.getdbt.com/docs/fusion/supported-features.md#features-and-capabilities)
