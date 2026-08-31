# What is dbt?

dbt transforms raw warehouse data into trusted data products. You write simple SQL select statements, and dbt handles the heavy lifting by creating modular, maintainable data models that power analytics, operations, and AI -- replacing the need for complex and fragile transformation code.

dbt is the industry standard for data transformation, helping teams work faster and produce higher-quality data. As you build in dbt, your project creates structured context — lineage, tests, contracts, metrics, and governance — that explains how your data connects, what it means, and what changes may affect.

That context makes dbt especially powerful for AI and comes with features like [dbt Wizard](#dbt-wizard), which helps you investigate, build, validate, and ship with full project context and governance on by default.

You can use dbt and its [framework](#dbt-framework) to:

* Centralize and modularize your analytics code, while also providing your data team with guardrails typically found in software engineering workflows.
* Collaborate on data models to safely deploy and monitor data transformations in production.
* Apply software engineering best practices like version control, testing, modularity, CI/CD, and documentation to analytics workflows.
* Build idempotent transformations that are safe to rerun and produce consistent results. Learn more about [Idempotence in dbt](../best-practices/idempotence.md).

Backed by a 100,000+ member [community](../community/join.md), dbt helps teams build high-quality, trustworthy data pipelines faster.

![dbt works alongside your ingestion, visualization, and other data tools, so you can transform data directly in your cloud data platform.](/img/docs/platform-overview.jpg?v=2 "dbt works alongside your ingestion, visualization, and other data tools, so you can transform data directly in your cloud data platform.")dbt works alongside your ingestion, visualization, and other data tools, so you can transform data directly in your cloud data platform.

Read more about why we want to enable analysts to work more like software engineers in [The dbt Viewpoint](../community/resources/viewpoint.md). Learn how other data practitioners around the world are using dbt by [joining the dbt Community](https://www.getdbt.com/community/join-the-community).

## dbt framework

Use the dbt framework to quickly and collaboratively transform data and deploy analytics code following software engineering best practices like version control, modularity, portability, CI/CD, and documentation. This means anyone on the data team familiar with SQL can safely contribute to production-grade data pipelines.

The dbt framework is composed of a *language* and an *engine*:

* The *dbt language* is the code you write in your dbt project — SQL select statements, Jinja templating, YAML configs, tests, and more. It's the standard for the data industry and the foundation of the dbt framework.

* The *dbt engine* compiles your project, executes your transformation graph, and produces metadata. Today, the current Rust-based version generation is v2. By default, installing dbt gives you SQL comprehension, editor features, and richer development workflows.

### dbt versions

dbt has two major versions: v1 and v2.

**v2** is the current generation of dbt and the default when you [install it](./local/install-dbt.md). Installing dbt comes with richer developer tooling, linting, and more. Refer to the [Upgrade v2 guide](./dbt-versions/dbt-upgrade/upgrading-to-v2.md) for more info.

| Version                   | What it is                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **v2**<br />*Recommended* | The current Rust-based generation of dbt, built for the full modern development experience and powered by an open-source runtime.<br /><br />Many features work right away. Some advanced capabilities unlock with a free sign-in — see [v2 feature availability](./dbt/dbt-availability.md) for the full breakdown or [upgrade to v2](./dbt-versions/dbt-upgrade/upgrading-to-v2.md). |
| **v1**                    | The original Python-based generation of dbt, still maintained as dbt Core v1.x.                                                                                                                                                                                                                                                                                                                                                              |

Refer to the [Licensing FAQs](https://www.getdbt.com/licenses-faq) for more info.

## The dbt engine

The current generation of the dbt is written in Rust with a native understanding of SQL across multiple engine dialects. That comprehension lets dbt catch errors before they reach your warehouse and powers editor features like autocomplete and inline errors as you type.

v2 is the default experience when you [install dbt](./local/install-dbt.md). It builds on the Apache 2.0 open-source runtime CLI foundation for the dbt framework. dbt is free to use, with some capabilities unlocked when you sign in with any dbt platform account.

### Enhance your development workflows

As a developer, dbt can:

* Immediately catch incorrect SQL in your dbt models, before they ever hit the warehouse
* Give you autocomplete, hover info, and inline errors as you type
* Preview inline CTEs for faster debugging
* Trace model and column definitions across your entire project

Get all of this, free, in the [dbt extension for VS Code](./about-dbt-extension.md), built on v2.

## How to use dbt

You can use dbt in different ways depending on your needs:

* [With the dbt platform](#dbt-platform) (recommended for most users)
* [Locally from your command line or code editor](#dbt-local-development)

### dbt platform

The dbt platform is the fastest way to run dbt: scheduling, CI/CD, documentation hosting, monitoring, and alerting, all in one place. It works with both v1 and v2, on every plan from Developer (free) through Enterprise+.

Develop directly in the platform with the [Studio IDE](./platform/studio-ide/develop-in-studio.md) or connect from your local machine with the dbt VS Code extension or dbt platform CLI.

Learn more about [dbt platform features](./platform/about-platform/dbt-platform-features.md), explore [plans and pricing](https://www.getdbt.com/pricing/), or try a [quickstart](https://docs.getdbt.com/guides).

### dbt local development

[Install dbt](./local/install-dbt.md) to run v2 locally from the command line, powered by an open-source runtime.

For the best development experience, we recommend pairing v2 with the [dbt VS Code extension](./about-dbt-extension.md) for autocomplete, inline errors, and lineage as you work. You can also run [`dbt login`](../reference/commands/login.md?version=2.0) to unlock additional capabilities and create a free dbt platform account.

To get started quickly, try the [dbt quickstart](../guides/dbt.md).

Other ways to run self-hosted dbt:

* [dbt Core v1.x](./local/install-dbt.md?version=1.0): The original Python-based CLI.
* [dbt Core v2.x](./local/install-dbt-v2.md): dbt Core 2.0, the free, fully open-source (Apache 2.0) distribution of the new Rust-based dbt engine. Typically for organizations with a strict requirement to use this OSS runtime.

To contribute to the open-source project, refer to the [GitHub repo](https://github.com/dbt-labs/dbt-core).

## Why use dbt

As a dbt user, your main focus will be on writing models (select queries) that reflect core business logic – there's no need to write boilerplate code to create tables and views, or to define the order of execution of your models. Instead, dbt handles turning these models into objects in your warehouse for you.

* **No boilerplate**: Write business logic with just a SQL `select` statement or a Python DataFrame. dbt handles materialization, transactions, DDL, and schema changes.
* **Modular and reusable**: Build data models that can be referenced in subsequent work. Change a model once and the change propagates to all its dependencies, so you can publish canonical business logic without reimplementing it.
* **Fast builds**: Use [incremental models](./build/incremental-models.md) and leverage metadata to optimize long-running models.
* **Tested and documented** — Write [data quality tests](./build/data-tests.md) on your underlying data and auto-generate [documentation](./build/documentation.md) alongside your code.
* **Software engineering workflows**: Version control, branching, pull requests, CI/CD, and [package management](./build/packages.md) for your data pipelines. Write DRYer code with [macros](./build/jinja-macros.md) and [hooks](./build/hooks-operations.md).
* **AI-powered development**: Use [dbt Wizard](./platform/wizard-overview.md) to investigate, build, validate, and ship from natural language. dbt Wizard is grounded in your project's full context, validates its own work against lineage and tests, and includes governance and audit trails by default.

## Related docs

* [Quickstarts for dbt](https://docs.getdbt.com/guides)
* [Best practice guides](../best-practices.md)
* [What is a dbt project?](./build/projects.md)
* [Supported features matrix](./dbt/supported-features.md)
* [AI and agents](./dbt-ai/about-dbt-ai.md)
* [Licensing](./dbt-licensing.md)
* [v2 license agreement](https://www.getdbt.com/dbt-fusion-engine-license-agreement)
