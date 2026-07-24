# What is dbt?

dbt transforms raw warehouse data into trusted data products. You write simple SQL select statements, and dbt handles the heavy lifting by creating modular, maintainable data models that power analytics, operations, and AI -- replacing the need for complex and fragile transformation code.

dbt is the industry standard for data transformation, helping teams work faster and produce higher-quality data. As you build in dbt, your project creates structured context — lineage, tests, contracts, metrics, and governance — that explains how your data connects, what it means, and what changes may affect.

That context makes dbt especially powerful for AI and comes with features like [dbt Wizard](#dbt-wizard), which helps you investigate, build, validate, and ship with full project context and governance on by default.

You can use dbt and its [framework](#dbt-framework) to:

* Centralize and modularize your analytics code, while also providing your data team with guardrails typically found in software engineering workflows.
* Collaborate on data models to safely deploy and monitor data transformations in production.
* Apply software engineering best practices like version control, testing, modularity, CI/CD, and documentation to analytics workflows.
* Build idempotent transformations that are safe to rerun and produce consistent results. Learn more about [Idempotence in dbt](https://docs.getdbt.com/best-practices/idempotence.md).

Backed by a 100,000+ member [community](https://docs.getdbt.com/community/join.md), dbt helps teams build high-quality, trustworthy data pipelines faster.

[![dbt works alongside your ingestion, visualization, and other data tools, so you can transform data directly in your cloud data platform.](/img/docs/platform-overview.jpg?v=2 "dbt works alongside your ingestion, visualization, and other data tools, so you can transform data directly in your cloud data platform.")](#)dbt works alongside your ingestion, visualization, and other data tools, so you can transform data directly in your cloud data platform.

Read more about why we want to enable analysts to work more like software engineers in [The dbt Viewpoint](https://docs.getdbt.com/community/resources/viewpoint.md). Learn how other data practitioners around the world are using dbt by [joining the dbt Community](https://www.getdbt.com/community/join-the-community).

## dbt framework[​](#dbt-framework "Direct link to dbt framework")

<!-- -->

Use the dbt framework to quickly and collaboratively transform data and deploy analytics code following software engineering best practices like version control, modularity, portability, CI/CD, and documentation. This means anyone on the data team familiar with SQL can safely contribute to production-grade data pipelines.

The dbt framework is composed of a *language* and an *engine*:

* The *dbt language* is the code you write in your dbt project — SQL select statements, Jinja templating, YAML configs, tests, and more. It's the standard for the data industry and the foundation of the dbt framework.

* The *dbt engine* compiles your project, executes your transformation graph, and produces metadata. Today, the current Rust-based version generation is v2. By default, installing dbt gives you SQL comprehension, editor features, and richer development workflows.

### dbt versions[​](#dbt-versions "Direct link to dbt versions")

dbt has two major versions: v1 and v2.

**v2** is the current generation of dbt and the default when you [install it](https://docs.getdbt.com/docs/local/install-dbt.md). Installing dbt comes with richer developer tooling, linting, and more. Refer to the [Upgrade v2 guide](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md) for more info.

| Version                   | What it is                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **v2**<br />*Recommended* | The current Rust-based generation of dbt, built for the full modern development experience and powered by an open-source runtime.<br /><br />Many features work right away. Some advanced capabilities unlock with a free sign-in — see [Fusion availability](https://docs.getdbt.com/docs/fusion/fusion-availability.md) for the full breakdown or [upgrade to v2](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md). |
| **v1**                    | The original Python-based generation of dbt, still maintained as dbt Core v1.x.                                                                                                                                                                                                                                                                                                                                                                 |

Refer to the [Licensing FAQs](https://www.getdbt.com/licenses-faq) for more info.

## How to use dbt[​](#how-to-use-dbt "Direct link to How to use dbt")

You can use dbt in different ways depending on your needs:

* [With the dbt platform](#dbt-platform) (recommended for most users)
* [Locally from your command line or code editor](#dbt-local-development)

### dbt platform[​](#dbt-platform "Direct link to dbt platform")

The dbt platform is the fastest way to run dbt: scheduling, CI/CD, documentation hosting, monitoring, and alerting, all in one place. It works with both v1 and v2, on every plan from Developer (free) through Enterprise+.

Develop directly in the platform with the [Studio IDE](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md) or connect from your local machine with the dbt VS Code extension or dbt CLI.

Learn more about [dbt platform features](https://docs.getdbt.com/docs/platform/about-platform/dbt-platform-features.md), explore [plans and pricing](https://www.getdbt.com/pricing/), or try a [quickstart](https://docs.getdbt.com/guides.md).

### dbt local development[​](#dbt-local-development "Direct link to dbt local development")

[Install dbt](https://docs.getdbt.com/docs/local/install-dbt.md) to run v2 locally from the command line, powered by an open-source runtime.

For the best development experience, we recommend pairing v2 with the [dbt VS Code extension](https://docs.getdbt.com/docs/about-dbt-extension.md) for autocomplete, inline errors, and lineage as you work. You can also run [`dbt login`](https://docs.getdbt.com/reference/commands/login.md) to unlock additional capabilities and create a free dbt platform account.

Other ways to run self-hosted dbt:

* [dbt Core v1.x](https://docs.getdbt.com/docs/local/install-dbt.md?version=1.0): The original Python-based CLI.
* [dbt Core v2.x](https://docs.getdbt.com/docs/local/install-dbt-core-v2.md): dbt Core 2.0, the free, fully open-source (Apache 2.0) distribution of the new Rust-based dbt engine. Typically for organizations with a strict requirement to use this OSS runtime.

## Why use dbt[​](#why-use-dbt "Direct link to Why use dbt")

As a dbt user, your main focus will be on writing models (select queries) that reflect core business logic – there's no need to write boilerplate code to create tables and views, or to define the order of execution of your models. Instead, dbt handles turning these models into objects in your warehouse for you.

* **No boilerplate**: Write business logic with just a SQL `select` statement or a Python DataFrame. dbt handles materialization, transactions, DDL, and schema changes.
* **Modular and reusable**: Build data models that can be referenced in subsequent work. Change a model once and the change propagates to all its dependencies, so you can publish canonical business logic without reimplementing it.
* **Fast builds**: Use [incremental models](https://docs.getdbt.com/docs/build/incremental-models.md) and leverage metadata to optimize long-running models.
* **Tested and documented** — Write [data quality tests](https://docs.getdbt.com/docs/build/data-tests.md) on your underlying data and auto-generate [documentation](https://docs.getdbt.com/docs/build/documentation.md) alongside your code.
* **Software engineering workflows**: Version control, branching, pull requests, CI/CD, and [package management](https://docs.getdbt.com/docs/build/packages.md) for your data pipelines. Write DRYer code with [macros](https://docs.getdbt.com/docs/build/jinja-macros.md) and [hooks](https://docs.getdbt.com/docs/build/hooks-operations.md).
* **AI-powered development**: Use [dbt Wizard](https://docs.getdbt.com/docs/platform/wizard-overview.md) to investigate, build, validate, and ship from natural language. dbt Wizard is grounded in your project's full context, validates its own work against lineage and tests, and includes governance and audit trails by default.

## Related docs[​](#related-docs "Direct link to Related docs")

* [Quickstarts for dbt](https://docs.getdbt.com/guides.md)
* [Best practice guides](https://docs.getdbt.com/best-practices.md)
* [What is a dbt project?](https://docs.getdbt.com/docs/build/projects.md)
* [AI and agents](https://docs.getdbt.com/docs/dbt-ai/about-dbt-ai.md)
* [Licensing](https://docs.getdbt.com/docs/dbt-licensing.md)
