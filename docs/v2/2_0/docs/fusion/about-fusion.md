# About the dbt Fusion engine

<!-- -->

dbt is the industry standard for data transformation. The dbt Fusion engine enables dbt to operate at speed and scale like never before.

<!-- -->

The dbt Fusion engine shares the same dbt framework you already know — the same dbt language and project structure — while enabling you to work faster and deploy transformation workloads more efficiently.

### What is Fusion[​](#what-is-fusion "Direct link to What is Fusion")

Fusion is written in Rust and has a native understanding of SQL across multiple engine dialects — catching errors before they reach your warehouse and powering editor features like autocomplete and inline errors as you type.

Fusion is the default experience when you [install dbt](https://docs.getdbt.com/docs/local/install-dbt.md). It gives you the recommended v2 experience from the command line and builds on the Apache 2.0 runtime available as dbt Core 2.0. It's free to use, with some capabilities unlocked when you sign in with any dbt platform account — free, no paid plan required.

## Why use Fusion[​](#why-use-fusion "Direct link to Why use Fusion")

As a developer, Fusion can:

* Immediately catch incorrect SQL in your dbt models, before they ever hit the warehouse
* Give you autocomplete, hover info, and inline errors as you type
* Preview inline CTEs for faster debugging
* Trace model and column definitions across your entire project

Get all of this, free, in the [dbt extension for VSCode](https://docs.getdbt.com/docs/about-dbt-extension.md) — built on Fusion.

### Thread management[​](#thread-management "Direct link to Thread management")

The dbt Fusion engine manages parallelism differently than dbt Core v1.x. Rather than treating the `threads` setting as a strict limit on concurrent operations, Fusion optimizes parallelism based on each adapter's characteristics.

* **Snowflake and Databricks**: Fusion ignores user-set threads and automatically optimizes parallelism for maximum performance.
* **BigQuery and Redshift**: Fusion respects user-set threads to manage rate limits and concurrency constraints.

For BigQuery and Redshift, setting `--threads 0` or omitting the setting allows Fusion to dynamically optimize. Low thread values can significantly slow down performance on these platforms.

For more information, refer to [Using threads](https://docs.getdbt.com/docs/running-a-dbt-project/using-threads.md#fusion-engine-thread-optimization).

### How to use Fusion[​](#how-to-use-fusion "Direct link to How to use Fusion")

You can use Fusion in three ways:

* Select Fusion from the version dropdown in the [dbt platform](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md#dbt-fusion-engine)
* [Install the dbt extension for VS Code](https://docs.getdbt.com/docs/install-dbt-extension.md)
* [Install dbt](https://docs.getdbt.com/docs/local/install-dbt.md) to get Fusion from the command line

To get started quickly, try the [Fusion quickstart](https://docs.getdbt.com/guides/fusion.md). If you use the dbt platform and want to keep local development in sync, refer to [Hybrid development with the dbt platform and Fusion](https://docs.getdbt.com/guides/fusion-platform-local-workflow.md).

*Need Apache 2.0 only? [Install dbt Core 2.0](https://docs.getdbt.com/docs/local/install-dbt-core-v2.md), the open-source project behind Fusion.*

<!-- -->

## More information about Fusion[​](#more-information-about-fusion "Direct link to More information about Fusion")

* [About the dbt extension](https://docs.getdbt.com/docs/about-dbt-extension.md)
* [Supported features matrix](https://docs.getdbt.com/docs/fusion/supported-features.md)
* [Install dbt](https://docs.getdbt.com/docs/local/install-dbt.md)
* [Quickstart for Fusion](https://docs.getdbt.com/guides/fusion.md?step=1)
* [Upgrade guide](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md)
* [Fusion license agreement](https://www.getdbt.com/dbt-fusion-engine-license-agreement)
