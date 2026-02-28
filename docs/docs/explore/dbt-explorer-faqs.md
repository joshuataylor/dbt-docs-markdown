# dbt Catalog FAQs

[Catalog](https://docs.getdbt.com/docs/explore/explore-projects.md) is dbt’s new knowledge base and lineage visualization experience. It offers an interactive and high-level view of your company’s entire data estate, where you can dive deep into the context you need to understand and improve lineage so your teams can trust the data they’re using to make decisions.

## Overview[​](#overview "Direct link to Overview")

 How does dbt Catalog help with data quality?

Catalog makes it easy and intuitive to understand your entire lineage — from data source to the reporting layer — so you can troubleshoot, improve, and optimize your pipelines. With built-in features like project recommendations and model performance analysis, you can be sure you have appropriate test and documentation coverage across your estate and quickly spot and remediate slow-running models. With column-level lineage, you can quickly identify the potential downstream impacts of table changes or work backwards to quickly understand the root cause of an incident. Catalog gives teams the insights they need to improve data quality proactively, ensuring pipelines stay performant and data trust remains solid.

 How is dbt Catalog priced?

Catalog is generally available to all regions and deployment types on all dbt [Enterprise-tier and Starter plans](https://www.getdbt.com/). Certain features within Catalog, such as project recommendations, multi-project lineage, column-level lineage, and more are only available on the Enterprise and Enterprise+ plans.

Catalog can be accessed by users with developer and read-only seats.

 What happened to dbt Docs?

Catalog is the default documentation experience for dbt customers. dbt Docs is still available but doesn't offer the same speed, metadata, or visibility as Catalog and will become a legacy feature.

## How dbt Catalog works[​](#how-dbt-catalog-works "Direct link to How dbt Catalog works")

 Can I use dbt Catalog on-premises or with my self-hosted dbt Core deployment?

No. Catalog and all of its features are only available as a dbt user experience. Catalog reflects the metadata from your dbt project(s) and their runs.

 How does dbt Catalog support dbt environments?

Catalog supports a production or staging [deployment environment](https://docs.getdbt.com/docs/deploy/deploy-environments.md) for each project you want to explore. It defaults to the latest production or staging state of a project. Users can only assign one production and one staging environment per dbt project.

Support for development (dbt CLI and Studio IDE) environments is coming soon.

 How do I get started in Catalog? How does it update?

Simply select **Catalog** from the dbt top navigation bar. Catalog automatically updates after each dbt run in the given project’s environment (production, by default). The dbt commands you run within the environment will generate and update the metadata in Catalog, so make sure to run the correct combination of commands within the jobs of the environment; for more details, refer to [Generate metadata](https://docs.getdbt.com/docs/explore/explore-projects.md#generate-metadata).

 Is it possible to export dbt lineage to an external system or catalog?

Yes. The lineage that powers Catalog is also available through the Discovery API.

 How does dbt Catalog integrate with third-party tools to show end-to-end lineage?

Catalog reflects all the lineage defined within the dbt project. Our vision for Catalog is to incorporate additional metadata from external tools like data loaders (sources) and BI/analytics tools (exposures) integrated with dbt, all seamlessly incorporated into the lineage of the dbt project.

 Why did previously visible data in dbt Catalog disappear?

Catalog automatically deletes stale metadata after 3 months if no jobs were run to refresh it. To avoid this, make sure you schedule jobs to run more frequently than 3 months with the necessary commands.

## Key features[​](#key-features "Direct link to Key features")

 Does dbt Catalog support multi-project discovery (dbt Mesh)?

Yes. Refer to [Explore multiple projects](https://docs.getdbt.com/docs/explore/explore-multiple-projects.md) to learn more.

 What kind of search capabilities does dbt Catalog support?

Resource search capabilities include using keywords, partial strings (fuzzy search), and set operators like `OR`. Meanwhile, lineage search supports using dbt selectors. For details, refer to [Keyword search](https://docs.getdbt.com/docs/explore/explore-projects.md#search-resources).

 Can I view model execution information for a job that is currently being run?

dbt updates the performance and metrics after a job run. However, **Model performance** charts only display data for *completed* UTC days. This means that runs from the current UTC day won't appear in the charts until the UTC day changes (midnight UTC). For example, if you're in US Pacific time, you won't see the current day's runs reflected until 4:00 PM PT.

 Can I analyze the number of successful model runs within a month?

A chart of models built by month is available in thedbt dashboard.

 Can model or column descriptions be edited within dbt?

Yes. Today, you can edit descriptions in the Studio IDE or dbt CLI by changing the YAML files within the dbt project. In the future, Catalog will support more ways of editing descriptions.

 Where do recommendations come from? Can they be customized?

Recommendations largely mirror the best practice rules from the `dbt_project_evaluator` package. At this time, recommendations can’t be customized. In the future, Catalog will likely support recommendation customization capabilities (for example, in project code).

## Column-level lineage[​](#column-level-lineage "Direct link to Column-level lineage")

 What are the best use cases for column-level lineage in dbt Catalog?

Column-level lineage in Catalog can be used to improve many data development workflows, including:

* **Audit** — Visualize how data moves through and is used in your dbt project
* **Root cause** — Improve time to detect and resolve data quality issues, tracking back to the source
* **Impact analysis** — Trace transformations and usage to avoid introducing issues for consumers
* **Efficiency** — Prune unnecessary columns to reduce costs and data team overhead

 Does the column-level lineage remain functional even if column names vary between models?

Yes. Column-level lineage can handle name changes across instances of the column in the dbt project.

 Can multiple projects leverage the same column definition?

No. Cross-project column lineage is supported in the sense of viewing how a public model is used across projects, but not on a column-level.

 Can column descriptions be propagated down in downstream lineage automatically?

Yes, a reused column, labeled as passthrough or rename, inherits its description from source and upstream model columns. In other words, source and upstream model columns propagate their descriptions downstream whenever they are not transformed, meaning you don’t need to manually define the description. Refer to [Inherited column descriptions](https://docs.getdbt.com/docs/explore/column-level-lineage.md#inherited-column-descriptions) for more info.

 Is column-level lineage also available in the development tab?

Not currently, but we plan to incorporate column-level awareness across features in dbt in the future.

## Availability, access, and permissions[​](#availability-access-and-permissions "Direct link to Availability, access, and permissions")

 How can non-developers interact with dbt Catalog?

Read-only users can consume metadata in Catalog. More bespoke experiences and exploration avenues for analysts and less-technical contributors will be provided in the future.

 Does dbt Catalog require a specific dbt plan?

Catalog is available on dbt Starter and all Enterprise plans. Certain features within Catalog, like project recommendations, multi-project lineage, column-level lineage, and more are only available on the Enterprise and Enterprise+ plans.

 Will dbt Core users be able to leverage any of these new dbt Catalog features?

No. Catalog is a dbt-only product experience.

 Is it possible to access dbt Catalog using a read-only license?

Yes, users with read-only access can use the Catalog. Specific feature availability within Catalog will depend on your dbt plan.

 Is there an easy way to share useful dbt Catalog content with people outside of dbt?

The ability to embed and share views is being evaluated as a potential future capability.

  Is dbt Catalog accessible from other areas inside dbt?

Yes, you can [access Catalog from various dbt features](https://docs.getdbt.com/docs/explore/access-from-dbt-cloud.md), ensuring you have a seamless experience navigating between resources and lineage in your project.

While the primary way to access Catalog is through the **Catalog** link in the navigation, you can also access it from the [Studio IDE](https://docs.getdbt.com/docs/explore/access-from-dbt-cloud.md#dbt-cloud-ide), [the lineage tab in jobs](https://docs.getdbt.com/docs/explore/access-from-dbt-cloud.md#lineage-tab-in-jobs), and the [model timing tab in jobs](https://docs.getdbt.com/docs/explore/access-from-dbt-cloud.md#model-timing-tab-in-jobs).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
