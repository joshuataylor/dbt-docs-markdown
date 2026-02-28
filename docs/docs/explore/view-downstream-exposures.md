# Visualize downstream exposures [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

Downstream exposures integrate natively with Tableau (Power BI coming soon) and auto-generate downstream lineage in Catalog for a richer experience.

As a data team, it’s critical that you have context into the downstream use cases and users of your data products. By leveraging downstream [exposures](https://docs.getdbt.com/docs/build/exposures.md) automatically, data teams can:

* Gain a better understanding of how models are used in downstream analytics, improving governance and decision-making.
* Reduce incidents and optimize workflows by linking upstream models to downstream dependencies.
* Automate exposure tracking for supported BI tools, ensuring lineage is always up to date.
* [Orchestrate exposures](https://docs.getdbt.com/docs/cloud-integrations/orchestrate-exposures.md) to refresh the underlying data sources during scheduled dbt jobs, improving timeliness and reducing costs. Orchestrating exposures is essentially a way to ensure that your BI tools are updated regularly by using the [dbt job scheduler](https://docs.getdbt.com/docs/deploy/deployments.md).
  <!-- -->
  * For more info on the differences between visualizing and orchestrating exposures, see [Visualize and orchestrate downstream exposures](https://docs.getdbt.com/docs/cloud-integrations/downstream-exposures.md).

To configure downstream exposures automatically from dashboards in Tableau, prerequisites, and more — refer to [Configure downstream exposures](https://docs.getdbt.com/docs/cloud-integrations/downstream-exposures-tableau.md).

### Supported plans[​](#supported-plans "Direct link to Supported plans")

Downstream exposures is available on all dbt [Enterprise-tier plans](https://www.getdbt.com/pricing/). Currently, you can only connect to a single Tableau site on the same server.

Tableau Server

If you're using Tableau Server, you need to [allowlist dbt's IP addresses](https://docs.getdbt.com/docs/cloud/about-cloud/access-regions-ip-addresses.md) for your dbt region.

<!-- -->

## View downstream exposures[​](#view-downstream-exposures "Direct link to View downstream exposures")

After setting up downstream exposures in dbt, you can view them in [Catalog](https://docs.getdbt.com/docs/explore/explore-projects.md) for a richer experience.

Navigate to Catalog by selecting **Catalog** from the top-level navigation. From the **Overview** page, you can view downstream exposures from a couple of places:

* [Exposures menu](#exposures-menu)
* [File tree](#file-tree)
* [Project lineage](#project-lineage)

### Exposures menu[​](#exposures-menu "Direct link to Exposures menu")

View downstream exposures from the **Exposures** menu item under **Resources**. This menu provides a comprehensive list of all the exposures so you can quickly access and manage them. The menu displays the following information:

* **Name**: The name of the exposure.
* **Health**: The [data health signal](https://docs.getdbt.com/docs/explore/data-health-signals.md) of the exposure.
* **Type**: The type of exposure, such as `dashboard` or `notebook`.
* **Owner**: The owner of the exposure.
* **Owner email**: The email address of the owner of the exposure.
* **Integration**: The BI tool that the exposure is integrated with.
* **Exposure mode**: The type of exposure defined: **Auto** or **Manual**.

[![View from the dbt Catalog under the project menu.](/img/docs/cloud-integrations/auto-exposures/explorer-view-resources.png?v=2 "View from the dbt Catalog under the project menu.")](#)View from the dbt Catalog under the project menu.

### File tree[​](#file-tree "Direct link to File tree")

Locate directly from within the **File tree** under the **imported\_from\_tableau** sub-folder. This view integrates exposures seamlessly with your project files, making it easy to find and reference them from your project's structure.

[![View from the dbt Catalog under the 'File tree' menu.](/img/docs/cloud-integrations/auto-exposures/explorer-view-file-tree.jpg?v=2 "View from the dbt Catalog under the 'File tree' menu.")](#)View from the dbt Catalog under the 'File tree' menu.

### Project lineage[​](#project-lineage "Direct link to Project lineage")

From the **Project lineage** view, which visualizes the dependencies and relationships in your project. Exposures are represented with the Tableau icon, offering an intuitive way to see how they fit into your project's overall data flow.

[![View from the dbt Catalog in your Project lineage view, displayed with the Tableau icon.](/img/docs/cloud-integrations/auto-exposures/explorer-lineage2.jpg?v=2 "View from the dbt Catalog in your Project lineage view, displayed with the Tableau icon.")](#)View from the dbt Catalog in your Project lineage view, displayed with the Tableau icon.

[![View from the dbt Catalog in your Project lineage view, displayed with the Tableau icon.](/img/docs/cloud-integrations/auto-exposures/explorer-lineage.jpg?v=2 "View from the dbt Catalog in your Project lineage view, displayed with the Tableau icon.")](#)View from the dbt Catalog in your Project lineage view, displayed with the Tableau icon.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
