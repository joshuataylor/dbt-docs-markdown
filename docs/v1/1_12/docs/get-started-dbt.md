# dbt Quickstarts

Begin your dbt journey by choosing how you want to develop:

* [**dbt platform** ](#the-dbt-platform)— Develop in your browser (Studio IDE or Canvas) or use local tools (VS Code extension, dbt platform CLI) that connect to your platform account. The platform provides hosted CI/CD, documentation, and more. Supports both the [dbt Fusion engine](./introduction.md) and [dbt Core](./local/install-dbt.md) engines.
* [**Self-hosted only**](#self-hosted-dbt-installations) — Use local tools like the [dbt VS Code extension](./about-dbt-extension.md) or [install dbt](./local/install-dbt.md) locally, to develop and run dbt on your own infrastructure. You can use local tools with or without a dbt platform account.
* **Local + dbt platform** — Use the VS Code extension or dbt platform CLI with a dbt platform account to develop locally while leveraging platform features like CI/CD, documentation hosting, Insights, Canvas, and more.
* [**dbt Wizard**](#dbt-wizard) — The AI agent for analytics engineering, available in the dbt platform and from your terminal. Grounded in your project's lineage, model health, and semantic definitions.

## The dbt platform

dbt provides a fully managed environment to develop, run, and deploy dbt projects—with CI/CD, documentation hosting, and more. Learn more about [dbt features](./platform/about-platform/dbt-platform-features.md) and [start your free trial](https://www.getdbt.com/signup/) today.

The dbt Fusion engine adds managed execution and a unified development experience so you can focus on building rather than infrastructure.

Choose your warehouse to get started with a quickstart:

[![](/img/icons/athena.svg)](https://docs.getdbt.com/guides/athena)

#### [Quickstart for dbt and Amazon Athena](https://docs.getdbt.com/guides/athena)

[Integrate dbt with Amazon Athena for your data transformations.](https://docs.getdbt.com/guides/athena)

[![](/img/icons/azure-synapse-analytics-2.svg)](https://docs.getdbt.com/guides/azure-synapse-analytics)

#### [Quickstart for dbt and Azure Synapse Analytics](https://docs.getdbt.com/guides/azure-synapse-analytics)

[Discover how to integrate dbt with Azure Synapse Analytics for your data transformations.](https://docs.getdbt.com/guides/azure-synapse-analytics)

[![](/img/icons/bigquery.svg)](https://docs.getdbt.com/guides/bigquery)

#### [Quickstart for dbt and BigQuery](https://docs.getdbt.com/guides/bigquery)

[Discover how to leverage dbt with BigQuery to streamline your analytics workflows.](https://docs.getdbt.com/guides/bigquery)

[![](/img/icons/databricks.svg)](https://docs.getdbt.com/guides/databricks)

#### [Quickstart for dbt and Databricks](https://docs.getdbt.com/guides/databricks)

[Learn how to integrate dbt with Databricks for efficient data processing and analysis.](https://docs.getdbt.com/guides/databricks)

[![](/img/icons/fabric.svg)](https://docs.getdbt.com/guides/microsoft-fabric)

#### [Quickstart for dbt and Microsoft Fabric](https://docs.getdbt.com/guides/microsoft-fabric)

[Explore the synergy between dbt and Microsoft Fabric to optimize your data transformations.](https://docs.getdbt.com/guides/microsoft-fabric)

[![](/img/icons/redshift.svg)](https://docs.getdbt.com/guides/redshift)

#### [Quickstart for dbt and Redshift](https://docs.getdbt.com/guides/redshift)

[Learn how to connect dbt to Redshift for more agile data transformations.](https://docs.getdbt.com/guides/redshift)

[![](/img/icons/snowflake.svg)](https://docs.getdbt.com/guides/snowflake)

#### [Quickstart for dbt and Snowflake](https://docs.getdbt.com/guides/snowflake)

[Unlock the full potential of using dbt with Snowflake for your data transformations.](https://docs.getdbt.com/guides/snowflake)

[![](/img/icons/starburst.svg)](https://docs.getdbt.com/guides/starburst-galaxy)

#### [Quickstart for dbt and Starburst Galaxy](https://docs.getdbt.com/guides/starburst-galaxy)

[Leverage dbt with Starburst Galaxy to enhance your data transformation workflows.](https://docs.getdbt.com/guides/starburst-galaxy)

[![](/img/icons/teradata.svg)](https://docs.getdbt.com/guides/teradata)

#### [Quickstart for dbt and Teradata](https://docs.getdbt.com/guides/teradata)

[Discover and use dbt with Teradata to enhance your data transformation workflows.](https://docs.getdbt.com/guides/teradata)

## Self-hosted dbt installations

When you install dbt into your Windows, macOS, or Linux environment, you get command-line tools and the VS Code extension that enable you to transform data using analytics engineering best practices.

You can use self-hosted tools with or without a dbt platform account. With an account, the VS Code extension and dbt platform CLI sync with your platform project for CI/CD, documentation, and more. Without an account, you run dbt entirely on your own infrastructure.

Develop with a self-hosted installation using the dbt Fusion engine or dbt Core engine.

[![](/img/icons/dbt-bit.svg)](../guides/dbt.md?step=2)

#### [Self-hosted dbt v2 from a manual install](../guides/dbt.md?step=2)

[Learn how to install self-hosted v2 and set up a project.](../guides/dbt.md?step=2)

[![](/img/icons/dbt-bit.svg)](../guides/manual-install.md)

#### [Self-hosted dbt v1 from a manual install](../guides/manual-install.md)

[Learn how to install self-hosted v1 and set up a project.](../guides/manual-install.md)

[![](/img/icons/duckdb-seeklogo.svg)](../guides/duckdb.md?step=1)

#### [Quickstart for dbt with DuckDB](../guides/duckdb.md?step=1)

[Learn how to connect dbt to DuckDB.](../guides/duckdb.md?step=1)

## dbt Wizard

[dbt Wizard](./platform/wizard-overview.md) is an AI agent purpose-built for analytics engineering. It uses dbt's [native metadata engine](./dbt-ai/about-dbt-ai.md) — a structured index of your project's lineage, model health, tests, and semantic definitions — to build, refactor, validate, and document your project grounded in full project context.

[![](/img/icons/dbt-copilot.svg)](./dbt-ai/wizard-ide.md)

#### [dbt Wizard in the dbt platform](./dbt-ai/wizard-ide.md)

[Use dbt Wizard in the Studio IDE or home app to build and refactor models from natural language, generate tests and docs, and validate changes against your warehouse.](./dbt-ai/wizard-ide.md)

[![](/img/icons/dbt-copilot.svg)](./dbt-ai/wizard-quickstart.md)

#### [dbt Wizard from your terminal](./dbt-ai/wizard-quickstart.md)

[Install the dbt Wizard CLI to run the agent locally against any dbt project — with or without a dbt platform plan. Start with a free trial using dbt managed AI, or bring your own provider key.](./dbt-ai/wizard-quickstart.md)

## Related docs

Expand your dbt knowledge and expertise with these additional resources:

* [Join the monthly demos](https://www.getdbt.com/resources/webinars/dbt-cloud-demos-with-experts) to see dbt in action and ask questions.
* [dbt AWS marketplace](https://aws.amazon.com/marketplace/pp/prodview-tjpcf42nbnhko) contains information on how to deploy dbt on AWS, user reviews, and more.
* [Best practices](../best-practices.md) contains information on how dbt Labs approaches building projects through our current viewpoints on structure, style, and setup.
* [dbt Learn](https://learn.getdbt.com) offers free online courses that cover dbt fundamentals, advanced topics, and more.
* [Join the dbt Community](https://www.getdbt.com/community/join-the-community) to learn how other data practitioners globally are using dbt, share your own experiences, and get help with your dbt projects.
