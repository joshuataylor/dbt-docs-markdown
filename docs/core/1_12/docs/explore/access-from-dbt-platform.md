# Access Catalog from dbt platform features

Access Catalog from jobs, Insights, and other areas of dbt to move quickly between run context, lineage, and resource metadata in your project.

This page describes how to open Catalog from orchestration and exploration workflows in dbt. The primary way to open Catalog is **Catalog** in the navigation; you can also open it from jobs and Insights as described in the sections below.

### Lineage tab in jobs[​](#lineage-tab-in-jobs "Direct link to Lineage tab in jobs")

The **Lineage tab** in dbt jobs displays the lineage associated with the [job run](https://docs.getdbt.com/docs/deploy/jobs.md). You can open Catalog directly from this tab to understand the dependencies and relationships of resources in your project.

#### Access Catalog from the lineage tab[​](#access-catalog-from-the-lineage-tab "Direct link to Access Catalog from the lineage tab")

* From a job, select the **Lineage tab**.
* Double-click a node in the lineage graph to open a new tab and view its metadata in Catalog.

[![Access dbt Catalog from the lineage tab by double-clicking on the lineage node.](/img/docs/collaborate/dbt-explorer/explorer-from-lineage.gif?v=2 "Access dbt Catalog from the lineage tab by double-clicking on the lineage node.")](#)Access dbt Catalog from the lineage tab by double-clicking on the lineage node.

### Model timing tab in jobs [Starter](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[​](#model-timing-tab-in-jobs- "Direct link to model-timing-tab-in-jobs-")

The [model timing tab](https://docs.getdbt.com/docs/deploy/run-visibility.md#model-timing) in dbt jobs displays the composition, order, and time taken by each model in a job run.

You can open Catalog from the **model timing tab** to investigate resources, diagnose performance bottlenecks, understand dependencies and relationships of slow-running models, and make changes to improve their performance.

#### Access Catalog from the model timing tab[​](#access-catalog-from-the-model-timing-tab "Direct link to Access Catalog from the model timing tab")

* From a job, select the **model timing tab**.
* Hover over a resource and select **View in Catalog** to open its metadata in Catalog.

[![Access dbt Catalog from the model timing tab by hovering over the resource and clicking 'View in Explorer'.](/img/docs/collaborate/dbt-explorer/explorer-from-model-timing.png?v=2 "Access dbt Catalog from the model timing tab by hovering over the resource and clicking 'View in Explorer'.")](#)Access dbt Catalog from the model timing tab by hovering over the resource and clicking 'View in Explorer'.

### dbt Insights [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[​](#dbt-insights- "Direct link to dbt-insights-")

Open Catalog from [Insights](https://docs.getdbt.com/docs/explore/access-dbt-insights.md) to view project lineage and resources such as tables, columns, metrics, dimensions, and more.

To open Catalog from Insights, select the **Catalog** icon in the Query console sidebar menu, then search for the resource you want.

[![dbt Insights integrated with dbt Catalog](/img/docs/dbt-insights/insights-explorer.png?v=2 "dbt Insights integrated with dbt Catalog")](#)dbt Insights integrated with dbt Catalog
