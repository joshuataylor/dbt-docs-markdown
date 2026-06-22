# Global navigation [Starter](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing") [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Search, explore, and analyze data assets across all your dbt projects and connected metadata sources. Discover cross-project lineage, data discovery, and unified analytics governance.

**Plan availability**

Global navigation search varies depending on your [dbt platform](https://www.getdbt.com/pricing) plan:

* Enterprise plans — Catalog lets you search across all [dbt resources](https://docs.getdbt.com/docs/build/projects.md) (models, seeds, snapshots, sources, exposures, and more) in your account, plus discover external metadata.
* Starter plans (single project) — Use global navigation to search and navigate resources within your project

## About Global navigation[​](#about-global-navigation "Direct link to About Global navigation")

Global navigation in Catalog lets you search, explore, and analyze data assets across all your dbt projects and connected metadata sources—giving you a unified, account-wide view of your analytics ecosystem. With global navigation, you can:

* Search data assets — expand your search by including dbt resources (models, seeds, snapshots, sources, exposures, and more) across your entire account. This broadens the results returned and gives you greater insight into all the assets across your dbt projects.
  <!-- -->
  * External metadata ingestion — connect directly to your data warehouse, giving you visibility into tables, views, and other resources that aren't defined in dbt with Catalog.

* Explore lineage — explore an interactive map of data relationships across all your dbt projects. It lets you:

  <!-- -->

  * View upstream/downstream dependencies for models, sources, and more.
  * Drill into project and column-level lineage, including multi-project (Mesh) links.
  * Filter with "lineage lenses" by resource type, materialization, layer, or run status.
  * Troubleshoot data issues by tracing root causes and downstream impacts.
  * Optimize pipelines by spotting slow, failing, or unused parts of your DAG.

* See recommendations — global navigation offers a project-wide snapshot of dbt health, highlighting actionable tips to enhance your analytics engineering. These insights are automatically generated using dbt metadata and best practices from the project evaluator ruleset.

* View model query history — see how often each dbt model is queried in your warehouse, helping you:

  <!-- -->

  * Track real usage via successful `SELECT`s (excluding builds/tests)
  * Identify most/least used models for optimization or deprecation
  * Guide investment and maintenance with data-driven insights

* Track downstream exposures — monitor how your dbt models and sources are used by BI tools, apps, ML models, and reports across all connected projects

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
