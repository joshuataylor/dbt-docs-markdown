# About dbt Insights [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

Learn how to query data with Insights and view documentation in Catalog.

Insights in dbt empowers users to seamlessly explore and query data with an intuitive, context-rich interface. It bridges technical and business users by combining metadata, documentation, AI-assisted tools, and powerful querying capabilities into one unified experience.

Insights in dbt integrates with [Catalog](https://docs.getdbt.com/docs/explore/explore-projects.md), [Studio IDE](https://docs.getdbt.com/docs/cloud/studio-ide/develop-in-studio.md), [Canvas](https://docs.getdbt.com/docs/cloud/canvas.md), [Copilot](https://docs.getdbt.com/docs/cloud/dbt-copilot.md), and [Semantic Layer](https://docs.getdbt.com/docs/use-dbt-semantic-layer/dbt-sl.md) to make it easier for you to perform exploratory data analysis, leverage AI-assisted tools, make faster decisions, and collaborate across teams.

[![Overview of the dbt Insights and its features](/img/docs/dbt-insights/insights-main.gif?v=2 "Overview of the dbt Insights and its features")](#)Overview of the dbt Insights and its features

## Key benefits[​](#key-benefits "Direct link to Key benefits")

Key benefits include:

* Quickly write, run, and iterate on SQL queries with tools like syntax highlighting, tabbed editors, and query history.
* Leverage dbt metadata, trust signals, and lineage from Catalog for informed query construction.
* Make data accessible to users of varied technical skill levels with SQL, Semantic Layer queries, and visual tools.
* Use Copilot's AI-assistance to generate or edit SQL queries, descriptions, and more.

Some example use cases include:

* Analysts can quickly construct queries to analyze sales performance metrics across regions and view results.
* All users have a rich development experience powered by Catalog's end-to-end exploration experience.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* Be on a dbt [Enterprise-tier](https://www.getdbt.com/pricing) plan — [book a demo](https://www.getdbt.com/contact) to learn more about Insights.

* Available on all [tenant](https://docs.getdbt.com/docs/cloud/about-cloud/tenancy.md) configurations.

* Have a dbt [developer license](https://docs.getdbt.com/docs/cloud/manage-access/seats-and-users.md) with access to Insights.

* Configured [developer credentials](https://docs.getdbt.com/docs/cloud/studio-ide/develop-in-studio.md#get-started-with-the-cloud-ide).

* Your production and development [environments](https://docs.getdbt.com/docs/dbt-cloud-environments.md) are on dbt’s ‘Latest’ [release track](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md) or a supported dbt version.

* Use a supported data platform: Snowflake, BigQuery, Databricks, Redshift, or Postgres.
  <!-- -->
  * Single sign-on (SSO) for development user accounts is supported. Deployment environments will be queried leveraging the user's development credentials.

* (Optional) — To query [Semantic Layer](https://docs.getdbt.com/docs/use-dbt-semantic-layer/dbt-sl.md) metrics from the Insights, you must also:

  <!-- -->

  * [Configure](https://docs.getdbt.com/docs/use-dbt-semantic-layer/setup-sl.md) the Semantic Layer for your dbt project.
  * Have a successful job run in the environment where you configured the Semantic Layer.

* (Optional) To enable [Language Server Protocol (LSP) features](https://docs.getdbt.com/docs/explore/navigate-dbt-insights.md#lsp-features-in-dbt-insights) in Insights and run your compilations on the dbt Fusion engine, set your development environment to use the **Latest Fusion** dbt version.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
