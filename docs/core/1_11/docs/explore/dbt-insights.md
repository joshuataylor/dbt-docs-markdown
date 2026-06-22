# About dbt Insights [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

Learn how to query data with Insights and view documentation in Catalog.

Insights in dbt empowers users to seamlessly explore and query data with an intuitive, context-rich interface. It bridges technical and business users by combining metadata, documentation, AI-assisted tools, and powerful querying capabilities into one unified experience.

Insights in dbt integrates with [Catalog](https://docs.getdbt.com/docs/explore/explore-projects.md), [Studio IDE](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md), [Canvas](https://docs.getdbt.com/docs/platform/canvas.md), [dbt Copilot in Insights](https://docs.getdbt.com/docs/explore/access-dbt-insights.md), and [Semantic Layer](https://docs.getdbt.com/docs/use-dbt-semantic-layer/dbt-sl.md) to make it easier for you to perform exploratory data analysis, leverage AI-assisted tools, make faster decisions, and collaborate across teams.

[![Overview of the dbt Insights and its features](/img/docs/dbt-insights/insights-main.gif?v=2 "Overview of the dbt Insights and its features")](#)Overview of the dbt Insights and its features

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* Be on a dbt [Enterprise or Enterprise+](https://www.getdbt.com/pricing) plan — [book a demo](https://www.getdbt.com/contact) to learn more about Insights.

* Available on all [tenant](https://docs.getdbt.com/docs/platform/about-platform/tenancy.md) configurations.

* Have a dbt [developer license](https://docs.getdbt.com/docs/platform/manage-access/seats-and-users.md) with access to Insights.

* Configured [developer credentials](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md#get-started-with-the-studio-ide).

* Your production and development [environments](https://docs.getdbt.com/docs/dbt-platform-environments.md) are on dbt’s ‘Latest’ [release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md) or a supported dbt version.

* Use a supported data platform: Snowflake, BigQuery, Databricks, Redshift, or Postgres.
  <!-- -->
  * Single sign-on (SSO) for development user accounts is supported. Deployment environments will be queried leveraging the user's development credentials.

* (Optional) — To query [Semantic Layer](https://docs.getdbt.com/docs/use-dbt-semantic-layer/dbt-sl.md) metrics from the Insights, you must also:

  <!-- -->

  * [Configure](https://docs.getdbt.com/docs/use-dbt-semantic-layer/setup-sl.md) the Semantic Layer for your dbt project.
  * Have a successful job run in the environment where you configured the Semantic Layer.

* (Optional) To enable [Language Server Protocol (LSP) features](https://docs.getdbt.com/docs/explore/navigate-dbt-insights.md#lsp-features-in-dbt-insights) in Insights and run your compilations on the dbt Fusion engine, set your development environment to use the **Fusion Stable** dbt version.

## Key benefits[​](#key-benefits "Direct link to Key benefits")

Key benefits include:

* Quickly write, run, and iterate on SQL queries with tools like syntax highlighting, tabbed editors, and query history.
* Leverage dbt metadata, trust signals, and lineage from Catalog for informed query construction.
* Make data accessible to users of varied technical skill levels with SQL, Semantic Layer queries, and visual tools.
* Use dbt Copilot in Insights to generate or edit SQL queries, descriptions, and more.

Some example use cases include:

* Analysts can quickly construct queries to analyze sales performance metrics across regions and view results.
* All users have a rich development experience powered by Catalog's end-to-end exploration experience.

<!-- -->

info

dbt Wizard is the new and recommended AI agent for governed data development in dbt. It handles the full development lifecycle — investigation, building, validation, and shipping — grounded in your dbt project's lineage, tests, contracts, and metric definitions.

dbt Copilot is separate from dbt Wizard and is dbt's inline AI assistance experience, providing single-click generation of SQL, documentation, tests, and semantic models in Studio IDE, Canvas, and Insights.

Refer to [dbt AI FAQs](https://docs.getdbt.com/docs/dbt-ai/dbt-ai-faqs.md#is-dbt-wizard-the-same-as-dbt-copilot), [Billing](https://docs.getdbt.com/docs/platform/billing.md), and [dbt's Terms of Use](https://www.getdbt.com/terms-of-use) for more information.
