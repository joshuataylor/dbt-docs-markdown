# View documentation

dbt provides intuitive and scalable tools for viewing your dbt documentation. Detailed documentation is essential for your developers and other stakeholders to gain shared context for your dbt project.

You can view documentation in three complementary ways, depending on your needs:

| Option                                                                  | Description                                                                                                                                                                                                                                                                                            | Availability                                  |
| ----------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------- |
| [**dbt Docs (Legacy)**](#dbt-docs)                                      | Generates a static website with model lineage, metadata, and documentation that can be hosted on your web server (like S3 or Netlify).                                                                                                                                                                 | dbt Core or dbt Developer plans               |
| [**dbt Docs v2**](#dbt-docs-v2) alpha                                   | A modern, performant open-source catalog built for data consumers. Includes a redesigned UI, large-project performance, Semantic Layer metadata, column-level lineage (Fusion), and a REST API for AI agents.                                                                                          | dbt Fusion engine and dbt Core 2.0            |
| [**Catalog**](https://docs.getdbt.com/docs/explore/explore-projects.md) | The premier documentation experience in dbt. Builds on dbt Docs to provide a dynamic, real-time interface with rich [metadata](https://docs.getdbt.com/docs/explore/explore-projects.md#generate-metadata), customizable views, deep insight into your project and resources, and collaborative tools. | dbt Starter, Enterprise, or Enterprise+ plans |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Navigating your documentation[​](#navigating-your-documentation "Direct link to Navigating your documentation")

The following sections describe how to navigate your documentation in Catalog and dbt Docs.

### Catalog [Starter](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[​](#catalog- "Direct link to catalog-")

[Catalog](https://docs.getdbt.com/docs/explore/explore-projects.md) offers a dynamic, interactive way to explore your models, sources, and lineage. To access Catalog, navigate to the **Catalog** option in the dbt navigation menu.

[![Example of Catalog's resource details page and its lineage.](/img/docs/collaborate/dbt-explorer/example-model-details.png?v=2 "Example of Catalog's resource details page and its lineage.")](#)Example of Catalog's resource details page and its lineage.

[![Access Catalog from the lineage tab in a job by double-clicking a lineage node.](/img/docs/collaborate/dbt-explorer/explorer-main-page.gif?v=2 "Access Catalog from the lineage tab in a job by double-clicking a lineage node.")](#)Access Catalog from the lineage tab in a job by double-clicking a lineage node.

Catalog offers users a comprehensive suite of features to enhance data project navigation and understanding, like:

* Interactive lineage visualization for your project's DAG to understand relationships between resources.
* Resource search bar with comprehensive filters to help find project resources efficiently and quickly.
* Model performance insights to access metadata on dbt runs for in-depth analysis of model performance and quality.
* Project recommendations with suggestions to improve test coverage and documentation across your data estate.
* Data health signals to monitor the health and performance of each resource through data health indicators.
* Model query history to track consumption queries on your models to gain deeper insights into data usage.
* Downstream exposures to automatically expose relevant data models from tools like Tableau to enhance visibility.

For additional details and instructions on how to explore your lineage, navigate your resources, view model query history and data health signals, feature availability, and more — refer to [Discover data with Catalog](https://docs.getdbt.com/docs/explore/explore-projects.md).

### dbt Docs v2 alpha[​](#dbt-docs-v2- "Direct link to dbt-docs-v2-")

dbt Docs v2 is the next-generation open-source catalog experience, available when using the dbt Fusion engine and dbt Core 2.0. It is designed for data consumers (analysts, BI users, data scientists, and stakeholders) who need to understand what data exists, how it was built, and whether they can trust it.

Key improvements over dbt Docs:

* **Performance:** Better handling for large dbt projects. The server reads from compact, pre-built index files rather than loading the full `manifest.json` in the browser.
* **Modernized UI:** Visually aligned with the dbt platform, with better navigation and resource discovery.
* **Semantic Layer metadata:** Surfaces compiled SQL logic, queryable dimensions, and metric definitions from your dbt Semantic Layer.
* **Column-level lineage:** Available when using the dbt Fusion engine.
* **REST API:** Exposes a `/api/v1/` interface so AI agents and external tooling can query metadata without a browser. This makes dbt Docs v2 a context source for MCP servers and coding agents.

To generate and serve dbt Docs v2, use the dbt Fusion engine or dbt Core 2.0 to build your project with `--write-index` (for example, `dbt compile --write-index`). Then, run `dbt docs serve`. Add [`--static-analysis strict`](https://docs.getdbt.com/docs/build/about-static-analysis?version=1.13) to also pull column lineage and richer column type metadata from your warehouse.

Refer to [dbt docs commands](https://docs.getdbt.com/reference/commands/cmd-docs.md) for full usage.

### dbt Docs (Legacy)[​](#dbt-docs-legacy "Direct link to dbt Docs (Legacy)")

dbt Docs provides valuable insights into your dbt Core or dbt Developer plan projects. The interface enables you to navigate to the documentation for specific models. That might look something like this:

[![Auto-generated documentation for a dbt model](/img/docs/building-a-dbt-project/testing-and-documentation/f2221dc-Screen_Shot_2018-08-14_at_6.29.55_PM.png?v=2 "Auto-generated documentation for a dbt model")](#)Auto-generated documentation for a dbt model

Here, you can see a representation of the project structure, a markdown description for a model, and a list of all of the columns (with documentation) in the model.

From the dbt Docs page, click the green button in the bottom-right corner of the webpage to expand a "mini-map" of your DAG. This pane displays the immediate parents and children of the model that you're exploring.

[![Opening the DAG mini-map](/img/docs/building-a-dbt-project/testing-and-documentation/ec77c45-Screen_Shot_2018-08-14_at_6.31.56_PM.png?v=2 "Opening the DAG mini-map")](#)Opening the DAG mini-map

In this example, the `fct_subscription_transactions` model only has one direct parent. By clicking the "Expand" button in the top-right corner of the window, we can pivot the graph horizontally and view the full lineage for our model. This lineage is filterable using the `--select` and `--exclude` flags, which are consistent with the semantics of [model selection syntax](https://docs.getdbt.com/reference/node-selection/syntax.md). Further, you can right-click to interact with the DAG, jump to documentation, or share links to your graph visualization with your coworkers.

[![The full lineage for a dbt model](/img/docs/building-a-dbt-project/testing-and-documentation/ac97fba-Screen_Shot_2018-08-14_at_6.35.14_PM.png?v=2 "The full lineage for a dbt model")](#)The full lineage for a dbt model

## Deploy the documentation site[​](#deploy-the-documentation-site "Direct link to Deploy the documentation site")

Effortlessly deploy documentation in Catalog or dbt Docs to make it available to your teams.

Security

The `dbt docs serve` command is only intended for local/development hosting of the documentation site. Please use one of the methods listed in the next section (or similar) to ensure that your documentation site is hosted securely!

### Catalog [Starter](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[​](#catalog--1 "Direct link to catalog--1")

Catalog automatically updates documentation after each production or staging job run using the metadata generated. This means it always has the latest results for your project with no manual deployment required. For details on how Catalog uses metadata to automatically update documentation, refer to [Generate metadata](https://docs.getdbt.com/docs/explore/explore-projects.md#generate-metadata).

To learn how to deploy your documentation site, see [Build and view your docs with dbt](https://docs.getdbt.com/docs/explore/build-and-view-your-docs.md).

### dbt Docs (Legacy)[​](#dbt-docs-legacy-1 "Direct link to dbt Docs (Legacy)")

dbt Docs was built to make it easy to host on the web. The site is "static," meaning you don't need any "dynamic" servers to serve the docs. You can host your documentation in several ways:

* Host on [Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html) (optionally [with IP access restrictions](https://docs.aws.amazon.com/AmazonS3/latest/dev/example-bucket-policies.html#example-bucket-policies-use-case-3))
* Publish with [Netlify](https://discourse.getdbt.com/t/publishing-dbt-docs-to-netlify/121)
* Use your own web server like Apache/Nginx
* If you're on a dbt Developer plan, see [Build and view your docs with dbt](https://docs.getdbt.com/docs/explore/build-and-view-your-docs.md#dbt-docs) to learn how to deploy your documentation site.

Interested in using Catalog for the complete dbt documentation experience, sign up for a free [dbt trial](https://www.getdbt.com/signup) or [contact us](https://www.getdbt.com/contact).
