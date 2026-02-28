# About the dbt Fusion engine

dbt is the industry standard for data transformation. The dbt Fusion engine enables dbt to operate at speed and scale like never before.

<!-- -->

The dbt Fusion engine shares the same familiar framework for authoring data transformations as dbt Core, while enabling data developers to work faster and deploy transformation workloads more efficiently.

### What is Fusion[​](#what-is-fusion "Direct link to What is Fusion")

Fusion is an entirely new piece of software, written in a different programming language (Rust) than dbt Core (Python). Fusion is significantly faster than dbt Core, and it has a native understanding of SQL across multiple engine dialects. Fusion will eventually support the full dbt Core framework, a superset of dbt Core’s capabilities, and the vast majority of existing dbt projects.

Fusion contains mixture of source-available, proprietary, and open source code. That means:

* dbt Labs publishes much of the source code in the [`dbt-fusion` repository](https://github.com/dbt-labs/dbt-fusion), where you can read the code and participate in community discussions.
* Some Fusion capabilities are exclusively available for paying customers of the cloud-based [dbt platform](https://www.getdbt.com/signup). Refer to [supported features](https://docs.getdbt.com/docs/fusion/supported-features.md#paid-features) for more information.

Read more about the licensing for the dbt Fusion engine [here](http://www.getdbt.com/licenses-faq).

## Why use Fusion[​](#why-use-fusion "Direct link to Why use Fusion")

As a developer, Fusion can:

* Immediately catch incorrect SQL in your dbt models
* Preview inline CTEs for faster debugging
* Trace model and column definitions across your dbt project

All of that and more is available in the [dbt extension for VSCode](https://docs.getdbt.com/docs/about-dbt-extension.md), with Fusion at the foundation.

Fusion also enables more-efficient deployments of large DAGs. By tracking which columns are used where, and which source tables have fresh data, Fusion can ensure that models are rebuilt only when they need to process new data. This ["state-aware orchestration"](https://docs.getdbt.com/docs/deploy/state-aware-about.md) is a feature of the dbt platform (formerly dbt Cloud).

### Thread management[​](#thread-management "Direct link to Thread management")

The dbt Fusion engine manages parallelism differently than dbt Core. Rather than treating the `threads` setting as a strict limit on concurrent operations, Fusion optimizes parallelism based on each adapter's characteristics.

* **Snowflake and Databricks**: Fusion ignores user-set threads and automatically optimizes parallelism for maximum performance.
* **BigQuery and Redshift**: Fusion respects user-set threads to manage rate limits and concurrency constraints.

For BigQuery and Redshift, setting `--threads 0` or omitting the setting allows Fusion to dynamically optimize. Low thread values can significantly slow down performance on these platforms.

For more information, refer to [Using threads](https://docs.getdbt.com/docs/running-a-dbt-project/using-threads.md#fusion-engine-thread-optimization).

### How to use Fusion[​](#how-to-use-fusion "Direct link to How to use Fusion")

You can:

* Select Fusion from the [dropdown/toggle in the dbt platform](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-version-in-cloud.md#dbt-fusion-engine) [Private preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [Install the dbt extension for VSCode](https://docs.getdbt.com/docs/install-dbt-extension.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [Install the Fusion CLI](https://docs.getdbt.com/docs/fusion/install-fusion-cli.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Go straight to the [Quickstart](https://docs.getdbt.com/guides/fusion.md) to *feel the Fusion* as fast as possible.

## What's next?[​](#whats-next "Direct link to What's next?")

dbt Labs launched the dbt Fusion engine as a public beta on May 28, 2025, with plans to reach full feature parity with dbt Core ahead of [Fusion's general availability](https://docs.getdbt.com/blog/dbt-fusion-engine-path-to-ga).

<!-- -->

## More information about Fusion[​](#more-information-about-fusion "Direct link to More information about Fusion")

Fusion marks a significant update to dbt. While many of the workflows you've grown accustomed to remain unchanged, there are a lot of new ideas, and a lot of old ones going away. The following is a list of the full scope of our current release of the Fusion engine, including implementation, installation, deprecations, and limitations:

* [About the dbt Fusion engine](https://docs.getdbt.com/docs/fusion/about-fusion.md)
* [About the dbt extension](https://docs.getdbt.com/docs/about-dbt-extension.md)
* [New concepts in Fusion](https://docs.getdbt.com/docs/fusion/new-concepts.md)
* [Supported features matrix](https://docs.getdbt.com/docs/fusion/supported-features.md)
* [Installing Fusion CLI](https://docs.getdbt.com/docs/fusion/install-fusion-cli.md)
* [Installing VS Code extension](https://docs.getdbt.com/docs/install-dbt-extension.md)
* [Fusion release track](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-version-in-cloud.md#dbt-fusion-engine)
* [Quickstart for Fusion](https://docs.getdbt.com/guides/fusion.md?step=1)
* [Upgrade guide](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-fusion.md)
* [Fusion licensing](http://www.getdbt.com/licenses-faq)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
