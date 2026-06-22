# Visualize downstream exposures [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

Downstream exposures integrate natively with Tableau (Power BI coming soon) and auto-generate downstream lineage in Catalog for a richer experience.

As a data team, it’s critical that you have context into the downstream use cases and users of your data products. By leveraging downstream [exposures](https://docs.getdbt.com/docs/build/exposures.md) automatically, data teams can:

* Gain a better understanding of how models are used in downstream analytics, improving governance and decision-making.
* Reduce incidents and optimize workflows by linking upstream models to downstream dependencies.
* Automate exposure tracking for supported BI tools, ensuring lineage is always up to date.
* [Orchestrate exposures](https://docs.getdbt.com/docs/platform-integrations/orchestrate-exposures.md) to refresh the underlying data sources during scheduled dbt jobs, improving timeliness and reducing costs. Orchestrating exposures is essentially a way to ensure that your BI tools are updated regularly by using the [dbt job scheduler](https://docs.getdbt.com/docs/deploy/deployments.md).
  <!-- -->
  * For more info on the differences between visualizing and orchestrating exposures, see [Visualize and orchestrate downstream exposures](https://docs.getdbt.com/docs/platform-integrations/downstream-exposures.md).

To configure downstream exposures automatically from dashboards in Tableau, prerequisites, and more — refer to [Configure downstream exposures](https://docs.getdbt.com/docs/platform-integrations/downstream-exposures-tableau.md).

### Supported plans[​](#supported-plans "Direct link to Supported plans")

Downstream exposures is available on all dbt [Enterprise-tier plans](https://www.getdbt.com/pricing/). Currently, you can only connect to a single Tableau site on the same server.

Tableau Server

If you're using Tableau Server, you need to [allowlist dbt's IP addresses](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md) for your dbt region.

<!-- -->

## View downstream exposures[​](#view-downstream-exposures "Direct link to View downstream exposures")

After setting up downstream exposures in dbt, you can view them in [Catalog](https://docs.getdbt.com/docs/explore/explore-projects.md) for a richer experience.

Navigate to Catalog by selecting **Catalog** from the top-level navigation. You can view downstream exposures from a couple of places:

* [Exposures menu](#exposures-menu)
* [Exposure detail page](#exposure-detail-page)
* [Project lineage](#project-lineage)

### Exposures menu[​](#exposures-menu "Direct link to Exposures menu")

View all downstream exposures for a project from the Catalog sidebar:

1. In the sidebar, select your project.
2. Under the project, select **Exposure**. You will only see this option if you set up downstream exposures in [Tableau](https://docs.getdbt.com/docs/platform-integrations/downstream-exposures-tableau.md#set-up-in-tableau) and [dbt](https://docs.getdbt.com/docs/platform-integrations/downstream-exposures-tableau.md#set-up-in-dbt-cloud).

The **Exposures** table lists every exposure in the project so you can quickly access and manage them:

* **Name**: The name of the exposure.
* **Health**: The [data health signal](https://docs.getdbt.com/docs/explore/data-health-signals.md) of the exposure.
* **Type**: The type of exposure, such as `dashboard` or `notebook`.
* **Owner**: The owner of the exposure.
* **Owner email**: The email address of the owner of the exposure.
* **Integration**: The BI tool that the exposure is integrated with.
* **Exposure mode**: The type of exposure defined: **Auto** or **Manual**. Auto exposures from Tableau appear alongside manual exposures

[![View the Exposures table from the Catalog sidebar.](/img/docs/platform-integrations/auto-exposures/explorer-view-resources.png?v=2 "View the Exposures table from the Catalog sidebar.")](#)View the Exposures table from the Catalog sidebar.

### Exposure detail page[​](#exposure-detail-page "Direct link to Exposure detail page")

After you open the **Exposures** table ([previous section](#exposures-menu)), select an exposure name to open its detail page.

On the exposure detail page, you can review metadata, [data health signals](https://docs.getdbt.com/docs/explore/data-health-signals.md), description, and lineage. For Tableau auto exposures, use **Open in Dashboard** to open the workbook in Tableau, or **Modify integration** to update your Tableau connection settings.

[![View an exposure detail page in Catalog.](/img/docs/platform-integrations/auto-exposures/explorer-view-exposure-detail.png?v=2 "View an exposure detail page in Catalog.")](#)View an exposure detail page in Catalog.

### Project lineage[​](#project-lineage "Direct link to Project lineage")

You can also view exposures from the **Project lineage** view, separate from the **Exposures** table:

1. In the sidebar, select your project.
2. Click **View lineage**.
3. Select an exposure node with the Tableau icon to view its details in the side panel.

This view visualizes the dependencies and relationships in your project. For Tableau auto exposures, use **View in Tableau** or **Modify integration** from the side panel.

[![View from the dbt Catalog in your Project lineage view, displayed with the Tableau icon.](/img/docs/platform-integrations/auto-exposures/explorer-lineage2.png?v=2 "View from the dbt Catalog in your Project lineage view, displayed with the Tableau icon.")](#)View from the dbt Catalog in your Project lineage view, displayed with the Tableau icon.

[![View from the dbt Catalog in your Project lineage view, displayed with the Tableau icon.](/img/docs/platform-integrations/auto-exposures/explorer-lineage.png?v=2 "View from the dbt Catalog in your Project lineage view, displayed with the Tableau icon.")](#)View from the dbt Catalog in your Project lineage view, displayed with the Tableau icon.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
