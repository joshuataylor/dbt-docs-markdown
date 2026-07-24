# Visualize and orchestrate downstream exposures

dbt platform | Enterprise, Enterprise+ⓘ

Visualize and orchestrate downstream exposures in dbt to automatically generate exposures from dashboards and proactively refresh the underlying data sources (like Tableau extracts) during scheduled dbt jobs.

<!-- -->

The following table summarizes the differences between visualizing and orchestrating downstream exposures:

| Info              | Set up and visualize downstream exposures                                          | Orchestrate downstream exposures [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles") |
| ----------------- | ---------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Purpose           | Automatically brings downstream assets into your dbt lineage.                      | Proactively refreshes the underlying data sources during scheduled dbt jobs.                                                                                               |
| Benefits          | Provides visibility into data flow and dependencies.                               | Ensures BI tools always have up-to-date data without manual intervention.                                                                                                  |
| Location          | Exposed in dbt [Catalog](https://docs.getdbt.com/docs/explore/explore-projects.md) | Exposed in [dbt scheduler](https://docs.getdbt.com/docs/deploy/deployments.md)                                                                                             |
| Supported BI tool | Tableau                                                                            | Tableau                                                                                                                                                                    |
| Use case          | Helps users understand how models are used and reduces incidents.                  | Optimizes timeliness and reduces costs by running models when needed.                                                                                                      |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Check out the following sections for more information on visualizing and orchestrating downstream exposures:

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/platform-integrations/downstream-exposures-tableau.md)

#### [Set up and visualize downstream exposures](https://docs.getdbt.com/docs/platform-integrations/downstream-exposures-tableau.md)

[Set up downstream exposures automatically from dashboards to understand how models are used in downstream tools for a richer downstream lineage.](https://docs.getdbt.com/docs/platform-integrations/downstream-exposures-tableau.md)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/platform-integrations/orchestrate-exposures.md)

#### [Orchestrate downstream exposures](https://docs.getdbt.com/docs/platform-integrations/orchestrate-exposures.md)

[Proactively refreshes the underlying data sources (like Tableau extracts) using the dbt scheduler during scheduled dbt jobs.](https://docs.getdbt.com/docs/platform-integrations/orchestrate-exposures.md)
