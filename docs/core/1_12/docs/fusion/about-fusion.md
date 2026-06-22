# About the dbt Fusion engine

dbt is the industry standard for data transformation. The dbt Fusion engine enables dbt to operate at speed and scale like never before.

important

The dbt Fusion engine is currently available for installation in:

* [Local command line interface (CLI) tools](https://docs.getdbt.com/docs/local/install-dbt.md?version=2) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [VS Code and Cursor with the dbt extension](https://docs.getdbt.com/docs/install-dbt-extension.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [dbt platform environments](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md#dbt-fusion-engine)

Join the conversation in our Community Slack channel [`#dbt-fusion-engine`](https://getdbt.slack.com/archives/C088YCAB6GH).

Read the [Fusion Diaries](https://github.com/dbt-labs/dbt-core/discussions/categories/announcements?discussions_q=is:open+diaries+category:Announcements) for the latest updates.

The dbt Fusion engine shares the same familiar framework for authoring data transformations as dbt Core, while enabling data developers to work faster and deploy transformation workloads more efficiently.

### What is Fusion[​](#what-is-fusion "Direct link to What is Fusion")

Fusion is written in Rust and has a native understanding of SQL across multiple engine dialects. Fusion will eventually support the full dbt Core framework, a superset of dbt Core capabilities, and the vast majority of existing dbt projects.

[dbt Core v2](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md) is the open-source Apache 2.0 foundation that Fusion builds on. dbt Core v2 delivers a faster, Rust-based runtime for dbt projects; Fusion extends it with SQL comprehension, column-level lineage, and richer development capabilities. It is currently in alpha.

dbt Core v2 is published as Apache 2.0 open source in the [`dbt-core` repository](https://github.com/dbt-labs/dbt-core), the single canonical home for issues and contributions. Fusion, which includes the dbt Fusion engine’s richer capabilities, is proprietary.

## Why use Fusion[​](#why-use-fusion "Direct link to Why use Fusion")

As a developer, Fusion can:

* Immediately catch incorrect SQL in your dbt models
* Preview inline CTEs for faster debugging
* Trace model and column definitions across your dbt project

All of that and more is available in the [dbt extension for VSCode](https://docs.getdbt.com/docs/about-dbt-extension.md), with Fusion at the foundation.

### Thread management[​](#thread-management "Direct link to Thread management")

The dbt Fusion engine manages parallelism differently than dbt Core. Rather than treating the `threads` setting as a strict limit on concurrent operations, Fusion optimizes parallelism based on each adapter's characteristics.

* **Snowflake and Databricks**: Fusion ignores user-set threads and automatically optimizes parallelism for maximum performance.
* **BigQuery and Redshift**: Fusion respects user-set threads to manage rate limits and concurrency constraints.

For BigQuery and Redshift, setting `--threads 0` or omitting the setting allows Fusion to dynamically optimize. Low thread values can significantly slow down performance on these platforms.

For more information, refer to [Using threads](https://docs.getdbt.com/docs/running-a-dbt-project/using-threads.md#fusion-engine-thread-optimization).

### How to use Fusion[​](#how-to-use-fusion "Direct link to How to use Fusion")

You can:

* Select Fusion from the [dropdown/toggle in the dbt platform](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md#dbt-fusion-engine)
* [Install the dbt extension for VSCode](https://docs.getdbt.com/docs/install-dbt-extension.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [Install the Fusion CLI](https://docs.getdbt.com/docs/local/install-dbt.md?version=2) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Go straight to the [Quickstart](https://docs.getdbt.com/guides/fusion.md) to *feel the Fusion* as fast as possible. If you're a dbt platform user and want to keep your local environment in sync with the platform, see the [Hybrid development with dbt platform and Fusion](https://docs.getdbt.com/guides/fusion-platform-local-workflow.md) guide.

## What's next?[​](#whats-next "Direct link to What's next?")

dbt Labs launched the dbt Fusion engine as a public beta on May 28, 2025, with plans to reach full feature parity with dbt Core ahead of [Fusion's general availability](https://docs.getdbt.com/blog/dbt-fusion-engine-path-to-ga).

<!-- -->

## More information about Fusion[​](#more-information-about-fusion "Direct link to More information about Fusion")

Fusion marks a significant update to dbt. While many of the workflows you've grown accustomed to remain unchanged, there are a lot of new ideas, and a lot of old ones going away. The following is a list of the full scope of our current release of the Fusion engine, including implementation, installation, deprecations, and limitations:

* [About the dbt Fusion engine](https://docs.getdbt.com/docs/fusion/about-fusion.md)
* [About the dbt extension](https://docs.getdbt.com/docs/about-dbt-extension.md)
* [New concepts in Fusion](https://docs.getdbt.com/docs/fusion/new-concepts.md)
* [Supported features matrix](https://docs.getdbt.com/docs/fusion/supported-features.md)
* [Installing Fusion CLI](https://docs.getdbt.com/docs/local/install-dbt.md?version=2)
* [Installing VS Code extension](https://docs.getdbt.com/docs/install-dbt-extension.md)
* [Fusion release track](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md#dbt-fusion-engine)
* [Quickstart for Fusion](https://docs.getdbt.com/guides/fusion.md?step=1)
* [Upgrade guide](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md)
* [Fusion license agreement](https://www.getdbt.com/dbt-fusion-engine-license-agreement)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
