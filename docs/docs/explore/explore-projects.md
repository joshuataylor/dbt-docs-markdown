# Discover data with Catalog

With Catalog, you can view your project's [resources](https://docs.getdbt.com/docs/build/projects.md) (such as models, tests, and metrics), their lineage, and [model consumption](https://docs.getdbt.com/docs/explore/view-downstream-exposures.md) to gain a better understanding of its latest production state.

Use Catalog to navigate and manage your projects within dbt to help you and other data developers, analysts, and consumers discover and leverage your dbt resources. Catalog integrates with the [Studio IDE](https://docs.getdbt.com/docs/cloud/studio-ide/develop-in-studio.md), [dbt Insights](https://docs.getdbt.com/docs/explore/dbt-insights.md), [Orchestrator](https://docs.getdbt.com/docs/deploy/deployments.md), and [Canvas](https://docs.getdbt.com/docs/cloud/canvas.md) to help you develop or view your dbt resources.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* You have a dbt account on the [Starter, Enterprise, or Enterprise+ plan](https://www.getdbt.com/pricing/).
* You have set up a [production](https://docs.getdbt.com/docs/deploy/deploy-environments.md#set-as-production-environment) or [staging](https://docs.getdbt.com/docs/deploy/deploy-environments.md#create-a-staging-environment) deployment environment for each project you want to explore.
* You have at least one successful job run in the deployment environment. Note that [CI jobs](https://docs.getdbt.com/docs/deploy/ci-jobs.md) do not update Catalog.
* You are on the Catalog page. To do this, select **Catalog** from the top-level navigation in dbt.

<!-- -->

## Generate metadata[​](#generate-metadata "Direct link to Generate metadata")

Catalog uses the metadata provided by the [Discovery API](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-api.md) to display the details about [the state of your dbt project](https://docs.getdbt.com/docs/dbt-cloud-apis/project-state.md). The metadata that's available depends on the [deployment environment](https://docs.getdbt.com/docs/deploy/deploy-environments.md) you've designated as *production* or *staging* in your dbt project.

Catalog also allows you to ingest metadata from your data warehouse, giving you visibility into external resources in Catalog. For information on supported warehouses, refer to [External metadata ingestion](https://docs.getdbt.com/docs/explore/external-metadata-ingestion.md#prerequisites).

## dbt metadata[​](#dbt-metadata "Direct link to dbt metadata")

If you're using a [hybrid project setup](https://docs.getdbt.com/docs/deploy/hybrid-setup.md) and uploading artifacts from dbt Core, make sure to follow the [setup instructions](https://docs.getdbt.com/docs/deploy/hybrid-setup.md#connect-project-in-dbt-cloud) to connect your project in dbt. This enables Catalog to access and display your metadata correctly.

* To ensure all metadata is available in Catalog, run `dbt build` and `dbt docs generate` as part of your job in your production or staging environment. Running these two commands ensures all relevant metadata (like lineage, test results, documentation, and more) is available in Catalog.
* Catalog automatically retrieves the metadata updates after each job run in the production or staging deployment environment so it always has the latest results for your project. This includes deploy and merge jobs.
  <!-- -->
  * Note that CI jobs don't update Catalog. This is because they don't reflect the production state and don't provide the necessary metadata updates.
* To view a resource and its metadata, you must define the resource in your project and run a job in the production or staging environment.
* The resulting metadata depends on the [commands](https://docs.getdbt.com/docs/deploy/job-commands.md) executed by the jobs.

### When dbt creates model metadata[​](#when-dbt-creates-model-metadata "Direct link to When dbt creates model metadata")

dbt populates a model's metadata in Catalog when both of the following conditions are met:

* The model is defined in your dbt project (it exists in the manifest).
* The model appears in the `run_results` of a [`dbt build`](https://docs.getdbt.com/reference/commands/build.md), [`dbt run`](https://docs.getdbt.com/reference/commands/run.md), or [`dbt clone`](https://docs.getdbt.com/reference/commands/clone.md) command, regardless of the run's success or failure status. Note that `dbt docs generate` alone does not create model entries in Catalog. It provides supplementary metadata like column details and descriptions for models that already exist.

### When dbt removes model metadata[​](#when-dbt-removes-model-metadata "Direct link to When dbt removes model metadata")

dbt removes a model's metadata from Catalog in these two cases:

* **Model removed from project**: If a model is deleted from your dbt project (and therefore no longer exists in the manifest), its metadata is removed after a subsequent job run in which the model is no longer included.
* **Environment inactivity**: If an environment has had no job runs in the past 3 months, all metadata for that environment is purged. To prevent this, schedule jobs to run at least once every 3 months.

| To view in Catalog                                        | You must successfully run                                                                                                                                                                                                                                                                                |
| --------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| All metadata                                              | [dbt build](https://docs.getdbt.com/reference/commands/build.md), [dbt docs generate](https://docs.getdbt.com/reference/commands/cmd-docs.md), and [dbt source freshness](https://docs.getdbt.com/reference/commands/source.md#dbt-source-freshness) together as part of the same job in the environment |
| Model lineage, details, or results                        | [dbt run](https://docs.getdbt.com/reference/commands/run.md) or [dbt build](https://docs.getdbt.com/reference/commands/build.md) on a given model within a job in the environment                                                                                                                        |
| Columns and statistics for models, sources, and snapshots | [dbt docs generate](https://docs.getdbt.com/reference/commands/cmd-docs.md) within [a job](https://docs.getdbt.com/docs/explore/build-and-view-your-docs.md) in the environment                                                                                                                          |
| Data test results                                         | [dbt test](https://docs.getdbt.com/reference/commands/test.md) or [dbt build](https://docs.getdbt.com/reference/commands/build.md) within a job in the environment                                                                                                                                       |
| Unit test results                                         | [dbt test](https://docs.getdbt.com/reference/commands/test.md) or [dbt build](https://docs.getdbt.com/reference/commands/build.md) within a job in the environment. Unit tests are typically run in development or CI environments, so their results rarely appear in production Catalog.                |
| Source freshness results                                  | [dbt source freshness](https://docs.getdbt.com/reference/commands/source.md#dbt-source-freshness) within a job in the environment                                                                                                                                                                        |
| Snapshot details                                          | [dbt snapshot](https://docs.getdbt.com/reference/commands/snapshot.md) or [dbt build](https://docs.getdbt.com/reference/commands/build.md) within a job in the environment                                                                                                                               |
| Seed details                                              | [dbt seed](https://docs.getdbt.com/reference/commands/seed.md) or [dbt build](https://docs.getdbt.com/reference/commands/build.md) within a job in the environment                                                                                                                                       |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

tip

If your organization works in both dbt Core and Cloud, you can unify these workflows by automatically uploading dbt Core artifacts into dbt Cloud and viewing them in Catalog for a more connected dbt experience. To learn more, visit [hybrid projects](https://docs.getdbt.com/docs/deploy/hybrid-projects.md).

### External metadata ingestion [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[​](#external-metadata-ingestion- "Direct link to external-metadata-ingestion-")

Connect directly to your data warehouse with [external metadata ingestion](https://docs.getdbt.com/docs/explore/external-metadata-ingestion.md), giving you visibility into tables, views, and other resources that aren't defined in dbt with Catalog.

We create dbt metadata and pull external metadata. Catalog uses the metadata provided by the [Discovery API](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-api.md) to display details about the state of your project. The available metadata depends on which [deployment environment](https://docs.getdbt.com/docs/deploy/deploy-environments.md) you’ve designated as production or staging in your dbt project.

## Catalog overview[​](#catalog-overview "Direct link to Catalog overview")

[Global navigation](https://docs.getdbt.com/docs/explore/global-navigation.md) [Starter](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

Catalog introduces the ability to widen your search by including dbt resources (models, seeds, snapshots, sources, exposures, and more) across your entire account. This broadens the results returned and gives you greater insight into all the assets across your dbt projects. Learn more in [Global navigation](https://docs.getdbt.com/docs/explore/global-navigation.md) or in our [video overview](https://www.loom.com/share/ae93b3d241cd439fbe5f98f5e6872113?).

Navigate the Catalog overview page to access your project's resources and metadata. The page includes the following sections:

* **Search bar** — [Search](#search-resources) for resources in your project by keyword. You can also use filters to refine your search results.
* **Sidebar** — Use the left sidebar to access model [performance](https://docs.getdbt.com/docs/explore/model-performance.md), [project recommendations](https://docs.getdbt.com/docs/explore/project-recommendations.md) in the **Project details** section. Browse your project's [resources, file tree, and database](#browse-with-the-sidebar) in the lower section of the sidebar.
  <!-- -->
  * Find your project recommendations within your project's landing page.\*
* **Lineage graph** — Explore your project's or account's [lineage graph](#project-lineage) to visualize the relationships between resources.
* **Latest updates** — View the latest changes or issues related to your project's resources, including the most recent job runs, changed properties, lineage, and issues.
* **Marts and public models** — View the [marts](https://docs.getdbt.com/best-practices/how-we-structure/1-guide-overview.md#guide-structure-overview) and [public models](https://docs.getdbt.com/docs/mesh/govern/model-access.md#access-modifiers) in your project. You can also navigate to all public models in your account through this view.
* **Model query history** — Use [model query history](https://docs.getdbt.com/docs/explore/model-query-history.md) to track consumption queries on your models for deeper insights.
* **Visualize downstream exposures** — [Set up](https://docs.getdbt.com/docs/cloud-integrations/downstream-exposures-tableau.md) and [visualize downstream exposures](https://docs.getdbt.com/docs/explore/view-downstream-exposures.md) to automatically expose relevant data models from Tableau to enhance visibility.
* **Data health signals** — View the [data-health-signals](https://docs.getdbt.com/docs/explore/data-health-signals.md) for each resource to understand its health and performance.

### Catalog permissions[​](#catalog-permissions "Direct link to Catalog permissions")

When using global navigation and searching across your projects, the following permissions apply.

* Your project access permissions determine which dbt projects appear in the left-hand menu of the global navigation.
* In Catalog searches, we use soft access controls, you'll see all matching resources in search results, with clear indicators for items you don't have access to.
* For external metadata, the global platform credential controls which resources metadata users can discover. See [External metadata ingestion](https://docs.getdbt.com/docs/explore/external-metadata-ingestion.md) for more details.

<!-- -->

On-demand learning

If you enjoy video courses, check out our [dbt Catalog on-demand course](https://learn.getdbt.com/courses/dbt-catalog) and learn how to best explore your dbt project(s)!

## Explore your project's lineage graph[​](#project-lineage "Direct link to Explore your project's lineage graph")

Catalog provides a visualization of your project's DAG that you can interact with. To access the project's full lineage graph, select **Overview** in the left sidebar and click the **Explore Lineage** button on the main (center) section of the page.

If you don't see the project lineage graph immediately, click **Render Lineage**. It can take some time for the graph to render depending on the size of your project and your computer's available memory. The graph of very large projects might not render so you can select a subset of nodes by using selectors, instead.

The nodes in the lineage graph represent the project's resources and the edges represent the relationships between the nodes. Nodes are color-coded and include iconography according to their resource type.

By default, Catalog shows the project's [applied state](https://docs.getdbt.com/docs/dbt-cloud-apis/project-state.md#definition-logical-vs-applied-state-of-dbt-nodes) lineage. That is, it shows models that have been successfully built and are available to query, not just the models defined in the project.

To explore the lineage graphs of tests and macros, view [their resource details pages](#view-resource-details). By default, Catalog excludes these resources from the full lineage graph unless a search query returns them as results.

 How can I interact with the full lineage graph?

* Hover over any item in the graph to display the resource's name and type.

* Zoom in and out on the graph by mouse-scrolling.

* Grab and move the graph and the nodes.

* Right-click on a node (context menu) to:

  * Refocus on the node, including its upstream and downstream nodes
  * Refocus on the node and its downstream nodes only
  * Refocus on the node and it upstream nodes only
  * View the node's [resource details](#view-resource-details) page

* Select a resource to highlight its relationship with other resources in your project. A panel opens on the graph's right-hand side that displays a high-level summary of the resource's details. The side panel includes a **General** tab for information like description, materialized type, and other details. In the side panel's upper right corner:

  * Click the View Resource icon to [view the resource details](#view-resource-details).
  * Click the [Open in IDE](#open-in-ide) icon to examine the resource using the [Studio IDE](https://docs.getdbt.com/docs/cloud/studio-ide/develop-in-studio.md).
  * Click the Copy Link to Page icon to copy the page's link to your clipboard.

* Use [selectors](https://docs.getdbt.com/reference/node-selection/methods.md) (in the search bar) to select specific resources or a subset of the DAG. This can help narrow the focus on the resources that interest you. All selectors are available for use, except those requiring a state comparison (result, source status, and state). You can also use the `--exclude` and the `--select` flag (which is optional). Examples:

  * `resource_type:model [RESOURCE_NAME]` — Returns all models matching the name search
  * `resource_type:metric,tag:nightly` — Returns metrics with the tag `nightly`

* Use [graph operators](https://docs.getdbt.com/reference/node-selection/graph-operators.md) (in the search bar) to select specific resources or a subset of the DAG. This can help narrow the focus on the resources that interest you. Examples:

  * `+orders` — Returns all the upstream nodes of `orders`
  * `+dim_customers,resource_type:source` — Returns all sources that are upstream of `dim_customers`

* Use [set operators](https://docs.getdbt.com/reference/node-selection/set-operators.md) (in the search bar) to select specific resources or a subset of the DAG. This can help narrow the focus on the resources that interest you. For example:

  * `+snowplow_sessions +fct_orders` — Use space-delineated arguments for a union operation. Returns resources that are upstream nodes of either `snowplow_sessions` or `fct_orders`.

* [View resource details](#view-resource-details) by selecting a node (double-clicking) in the graph.

* Click **Lenses** (lower right corner of the graph) to use Catalog [lenses](#lenses) feature.

### Example of full lineage graph[​](#example-of-full-lineage-graph "Direct link to Example of full lineage graph")

Example of exploring a model in the project's lineage graph:

[![Example of full lineage graph](/img/docs/collaborate/dbt-explorer/example-project-lineage-graph.png?v=2 "Example of full lineage graph")](#)Example of full lineage graph

## Lenses[​](#lenses "Direct link to Lenses")

The **Lenses** feature is available from your [project's lineage graph](#project-lineage) (lower right corner). Lenses are like map layers for your DAG. Lenses make it easier to understand your project's contextual metadata at scale, especially to distinguish a particular model or a subset of models.

When you apply a lens, tags become visible on the nodes in the lineage graph, indicating the layer value along with coloration based on that value. If you're significantly zoomed out, only the tags and their colors are visible in the graph.

Lenses are helpful to analyze a subset of the DAG if you're zoomed in, or to find models/issues from a larger vantage point.

 List of available lenses

A resource in your project is characterized by resource type, materialization type, or model layer, as well as its latest run or latest test status. Lenses are available for the following metadata:

* **Resource type**: Organizes resources by resource type, such as models, tests, seeds, saved query, and [more](https://docs.getdbt.com/docs/build/projects.md). Resource type uses the `resource_type` selector.

* **Materialization type**: Identifies the strategy for building the dbt models in your data platform.

* **Latest status**: The status from the latest execution of the resource in the current environment. For example, diagnosing a failed DAG region.

* **Model layer**: The modeling layer that the model belongs to according to [best practices guide](https://docs.getdbt.com/best-practices/how-we-structure/1-guide-overview.md#guide-structure-overview). For example, discovering marts models to analyze.

  <!-- -->

  * **Marts** — A model with the prefix `fct_` or `dim_` or a model that lives in the `/marts/` subdirectory.
  * **Intermediate** — A model with the prefix `int_`. Or, a model that lives in the `/int/` or `/intermediate/` subdirectory.
  * **Staging** — A model with the prefix `stg_`. Or, a model that lives in the `/staging/` subdirectory.

* **Test status**: The status from the latest execution of the tests that ran again this resource. In the case that a model has multiple tests with different results, the lens reflects the 'worst case' status.

* **Consumption query history**: The number of queries against this resource over a given time period.

### Example of lenses[​](#example-of-lenses "Direct link to Example of lenses")

Example of applying the **Materialization type** *lens* with the lineage graph zoomed out. In this view, each model name has a color according to the materialization type legend at the bottom, which specifies the materialization type. This color-coding helps to quickly identify the materialization types of different models.

[![Example of the Materialization type lens](/img/docs/collaborate/dbt-explorer/example-materialization-type.jpg?v=2 "Example of the Materialization type lens")](#)Example of the Materialization type lens

Example of applying the **Tests Status** *lens*, where each model name displays the tests status according to the legend at the bottom, which specifies the test status.

[![Example of the Test Status lens](/img/docs/collaborate/dbt-explorer/example-test-status.jpg?v=2 "Example of the Test Status lens")](#)Example of the Test Status lens

## Keyword search[​](#search-resources "Direct link to Keyword search")

With Catalog, global navigation provides a search experience allowing you to find dbt resources across all your projects, as well as non-dbt resources in Snowflake.

You can locate resources in your project by performing a keyword search in the search bar. All resource names, column names, resource descriptions, warehouse relations, and code matching your search criteria will be displayed as a list on the main (center) section of the page. When searching for an exact column name, the results show all relational nodes containing that column in their schemas. If there's a match, a notice in the search result indicates the resource contains the specified column. Also, you can apply filters to further refine your search results.

 Search features

* **Partial keyword search** — Also referred to as fuzzy search. Catalog uses a "contains" logic to improve your search results. This means you can search for partial terms without knowing the exact root word of your search term.
* **Exclude keywords** — Prepend a minus sign (-) to the keyword you want to exclude from search results. For example, `-user` will exclude all matches of that keyword from search results.
* **Boolean operators** — Use Boolean operators to enhance your keyword search. For example, the search results for `users OR github` will include matches for either keyword.
* **Phrase search** — Surround a string of keywords with double quotation marks to search for that exact phrase (for example, `"stg users"`). To learn more, refer to [Phrase search](https://en.wikipedia.org/wiki/Phrase_search) on Wikipedia.
* **SQL keyword search** — Use SQL keywords in your search. For example, the search results `int github users joined` will include matches that contain that specific string of keywords (similar to phrase searching).

 Filters side panel

The **Filters** side panel becomes available after you perform a keyword search. Use this panel to further refine the results from your keyword search. By default, Catalog searches across all resources in the project. You can filter on:

* [Resource type](https://docs.getdbt.com/docs/build/projects.md) (like models, sources, and so on)
* [Model access](https://docs.getdbt.com/docs/mesh/govern/model-access.md) (like public, private)
* [Model layer](https://docs.getdbt.com/best-practices/how-we-structure/1-guide-overview.md) (like marts, staging)
* [Model materialization](https://docs.getdbt.com/docs/build/materializations.md) (like view, table)
* [Tags](https://docs.getdbt.com/reference/resource-configs/tags.md) (supports multi-select)

Under the **Models** option, you can filter on model properties (access or materialization type). Also available are **Advanced** options, where you can limit the search results to column name, model code, and more.

 Global navigation

Catalog builds on the functionality of the old navigation and introduces exciting new capabilities to enhance your experience. For more information, refer to [Global navigation](https://docs.getdbt.com/docs/explore/global-navigation.md).

### Example of keyword search[​](#example-of-keyword-search "Direct link to Example of keyword search")

Example of results from searching on the keyword `customers` and applying the filters models, description, and code. [Data health signals](https://docs.getdbt.com/docs/explore/data-health-signals.md) are visible to the right of the model name in the search results.

## Browse with the sidebar[​](#browse-with-the-sidebar "Direct link to Browse with the sidebar")

From the sidebar, you can browse your project's resources, its file tree, and the database.

* **Resources** tab — All resources in the project organized by type. Select any resource type in the list and all those resources in the project will display as a table in the main section of the page. For a description on the different resource types (like models, metrics, and so on), refer to [About dbt projects](https://docs.getdbt.com/docs/build/projects.md).
  <!-- -->
  * [Data health signals](https://docs.getdbt.com/docs/explore/data-health-signals.md) are visible to the right of the resource name under the **Health** column.
* **File Tree** tab — All resources in the project organized by the file in which they are defined. This mirrors the file tree in your dbt project repository.
* **Database** tab — All resources in the project organized by the database and schema in which they are built. This mirrors your data platform's structure that represents the [applied state](https://docs.getdbt.com/docs/dbt-cloud-apis/project-state.md) of your project.

## Integrated tool access[​](#integrated-tool-access "Direct link to Integrated tool access")

Users with a [developer license](https://docs.getdbt.com/docs/cloud/manage-access/about-user-access.md#license-based-access-control) or an analyst seat can open a resource directly from the Catalog in the Studio IDE to view its model files, in Insights to query it, or in Canvas for visual editing.

## View model versions[​](#view-model-versions "Direct link to View model versions")

If models in the project are versioned, you can see which [version of the model](https://docs.getdbt.com/docs/mesh/govern/model-versions.md) is being applied — `prerelease`, `latest`, and `old` — in the title of the model's details page and in the model list from the sidebar.

## View resource details[​](#view-resource-details "Direct link to View resource details")

You can view the definition and latest run results of any resource in your project. To find a resource and view its details, you can interact with the lineage graph, use search, or browse the Catalog.

The details (metadata) available to you depends on the resource's type, its definition, and the [commands](https://docs.getdbt.com/docs/deploy/job-commands.md) that run within jobs in the production environment.

In the upper right corner of the resource details page, you can:

* Click the [Open in Studio IDE](#open-in-ide) icon to examine the resource using the [Studio IDE](https://docs.getdbt.com/docs/cloud/studio-ide/develop-in-studio.md).
* Click the Share icon to copy the page's link to your clipboard.

 What details are available for a model?

* **Data health signals** — [Data health signals](https://docs.getdbt.com/docs/explore/data-health-signals.md) offer a quick, at-a-glance view of data health. These icons indicate whether a model is Healthy, Caution, Degraded, or Unknown. Hover over an icon to view detailed information about the model's health.

* **Status bar** (below the page title) — Information on the last time the model ran, whether the run was successful, how the data is materialized, number of rows, and the size of the model.

* **General** tab includes:

  <!-- -->

  * **Lineage** graph — The model's lineage graph that you can interact with. The graph includes one upstream node and one downstream node from the model. Click the Expand icon in the graph's upper right corner to view the model in full lineage graph mode.
  * **Description** section — A [description of the model](https://docs.getdbt.com/docs/build/documentation.md#adding-descriptions-to-your-project).
  * **Recent** section — Information on the last time the model ran, how long it ran for, whether the run was successful, the job ID, and the run ID.
  * **Tests** section — [Data tests](https://docs.getdbt.com/docs/build/data-tests.md) for the model, including a status indicator for the latest test status. A
    <!-- -->
    ✅
    <!-- -->
    denotes a passing test.
  * **Details** section — Key properties like the model's relation name (for example, how it's represented and how you can query it in the data platform: `database.schema.identifier`); model governance attributes like access, group, and if contracted; and more.
  * **Relationships** section — The nodes the model **Depends On**, is **Referenced by**, and (if applicable) is **Used by** for projects that have declared the models' project as a dependency.

* **Code** tab — The source code and compiled code for the model.

* **Columns** tab — The available columns in the model. This tab also shows tests results (if any) that you can select to view the test's details page. A
  <!-- -->
  ✅
  <!-- -->
  denotes a passing test. To filter the columns in the resource, you can use the search bar that's located at the top of the columns view.

 What details are available for an exposure?

* **Status bar** (below the page title) — Information on the last time the exposure was updated.

* **Data health signals** — [Data health signals](https://docs.getdbt.com/docs/explore/data-health-signals.md) offer a quick, at-a-glance view of data health. These icons indicate whether a resource is Healthy, Caution, or Degraded. Hover over an icon to view detailed information about the exposure's health.

* **General** tab includes:

  <!-- -->

  * **Data health** — The status on data freshness and data quality.
  * **Status** section — The status on data freshness and data quality.
  * **Lineage** graph — The exposure's lineage graph. Click the **Expand** icon in the graph's upper right corner to view the exposure in full lineage graph mode. Integrates natively with Tableau and auto-generates downstream lineage.
  * **Description** section — A description of the exposure.
  * **Details** section — Details like exposure type, maturity, owner information, and more.
  * **Relationships** section — The nodes the exposure **Depends On**.

 What details are available for a test?

* **Status bar** (below the page title) — Information on the last time the test ran, whether the test passed, test name, test target, and column name. Defaults to all if not specified.
* **Test Type** (next to the Status bar) — Information on the different test types available: Unit test or Data test. Defaults to all if not specified.

When you select a test, the following details are available:

* **General** tab includes:

  <!-- -->

  * **Lineage** graph — The test's lineage graph that you can interact with. The graph includes one upstream node and one downstream node from the test resource. Click the Expand icon in the graph's upper right corner to view the test in full lineage graph mode.
  * **Description** section — A description of the test.
  * **Recent** section — Information on the last time the test ran, how long it ran for, whether the test passed, the job ID, and the run ID.
  * **Details** section — Details like schema, severity, package, and more.
  * **Relationships** section — The nodes the test **Depends On**.

* **Code** tab — The source code and compiled code for the test.

Example of the Tests view:

 What details are available for each source table within a source collection?

* **Status bar** (below the page title) — Information on the last time the source was updated and the number of tables the source uses.

* **Data health signals** — [Data health signals](https://docs.getdbt.com/docs/explore/data-health-signals.md) offer a quick, at-a-glance view of data health. These icons indicate whether a resource is Healthy, Caution, or Degraded. Hover over an icon to view detailed information about the source's health.

* **General** tab includes:

  <!-- -->

  * **Lineage** graph — The source's lineage graph that you can interact with. The graph includes one upstream node and one downstream node from the source. Click the Expand icon in the graph's upper right corner to view the source in full lineage graph mode.
  * **Description** section — A description of the source.
  * **Source freshness** section — Information on whether refreshing the data was successful, the last time the source was loaded, the timestamp of when a run generated data, and the run ID.
  * **Details** section — Details like database, schema, and more.
  * **Relationships** section — A table that lists all the sources used with their freshness status, the timestamp of when freshness was last checked, and the timestamp of when the source was last loaded.

* **Columns** tab — The available columns in the source. This tab also shows tests results (if any) that you can select to view the test's details page. A
  <!-- -->
  ✅
  <!-- -->
  denotes a passing test.

### Example of model details[​](#example-of-model-details "Direct link to Example of model details")

Example of the details view for the model `customers`:<br />

[![Example of resource details](/img/docs/collaborate/dbt-explorer/example-model-details.png?v=2 "Example of resource details")](#)Example of resource details

[![Example of downstream exposure details for Tableau.](/img/docs/cloud-integrations/auto-exposures/explorer-lineage2.jpg?v=2 "Example of downstream exposure details for Tableau.")](#)Example of downstream exposure details for Tableau.

## Staging environment[​](#staging-environment "Direct link to Staging environment")

Catalog supports views for [staging deployment environments](https://docs.getdbt.com/docs/deploy/deploy-environments.md#staging-environment), in addition to the production environment. This gives you a unique view into your pre-production data workflows, with the same tools available in production, while providing an extra layer of scrutiny.

You can explore the metadata from your production or staging environment to inform your data development lifecycle. Just [set a single environment](https://docs.getdbt.com/docs/deploy/deploy-environments.md) per dbt project as "production" or "staging," and ensure the proper metadata has been generated then you'll be able to view it in Catalog. Refer to [Generating metadata](https://docs.getdbt.com/docs/explore/explore-projects.md#generate-metadata) for more details.

## Related content[​](#related-content "Direct link to Related content")

* [Enterprise permissions](https://docs.getdbt.com/docs/cloud/manage-access/enterprise-permissions.md)
* [About model governance](https://docs.getdbt.com/docs/mesh/govern/about-model-governance.md)
* Blog on [What is data mesh?](https://www.getdbt.com/blog/what-is-data-mesh-the-definition-and-importance-of-data-mesh)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
