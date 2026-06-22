# Monitor dbt State activity [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Learn how to monitor dbt State activity in dbt platform for better visibility into model builds and cost savings.

dbt State monitoring helps you:

* **Track effectiveness of dbt State** — See how dbt State reduces unnecessary model rebuilds by only building models when there are changes to the data or code. dbt State provides transparency into how the optimization works across your projects.
* **Analyze build patterns** — Gain insights into your project's build frequency and identify opportunities for further optimization.

## dbt State metrics[​](#dbt-state-metrics "Direct link to dbt State metrics")

When you go to **Account settings** > **State**, the **dbt State** page shows how many days remain in your trial period. Once dbt State is enabled, it also displays the following for the current month:

* **Number of models reused**: How many model builds dbt State skipped or cloned instead of rebuilding from scratch.
* **Total % build reduction**: The overall reduction in model builds across your account.
* **Total query run time reduction**: The total time dbt State saved by not executing unnecessary model builds.

## Models built and reused chart[​](#models-built-and-reused-chart "Direct link to Models built and reused chart")

When you go to your **Account home**, you'll see a chart showing the number of models built and reused, giving you visibility into how dbt State is optimizing your data builds. You can also view the number of reused models per project on **Account home**.

## Logs view of built models[​](#logs-view-of-built-models "Direct link to Logs view of built models")

When you run a job, or when you run `dbt run` or `dbt build` locally, a structured logs view shows which models were built, skipped, or reused.

[![Logs view of built models](/img/docs/dbt-platform/using-dbt-platform/sao-logs-view.png?v=2 "Logs view of built models")](#)Logs view of built models

1. Each model has an icon indicating its status.
2. The **Reused** tab indicates the total number of reused models.
3. You can use the search bar or filter the logs to show **All**, **Success**, **Warning**, **Failed**, **Running**, **Skipped**, **Reused**, or **Debugged** messages.
4. Detailed log messages provide context on why models were built, reused, or skipped. These messages are highlighted in the logs.

## Reused tag in the Latest status lens[​](#reused-tag-in-the-latest-status-lens "Direct link to Reused tag in the Latest status lens")

Lineage lenses are interactive visual filters in [dbt Catalog](https://docs.getdbt.com/docs/explore/explore-projects.md#lenses) that show additional context on your lineage graph to understand how resources are defined or performing. When you apply a lens, tags become visible on the nodes in the lineage graph, indicating the layer value along with coloration based on that value. If you're significantly zoomed out, only the tags and their colors are visible in the graph.

The **Latest status** lens shows the status from the latest execution of the resource in the current environment. When you use this lens to view your lineage, dbt State tags reused models with **Reused**.

[![Latest status lens showing reused models](/img/docs/dbt-platform/using-dbt-platform/sao-latest-status-lens.png?v=2 "Latest status lens showing reused models")](#)Latest status lens showing reused models

To view your lineage with the **Latest status** lens:

1. From the main menu, go to **Orchestration** > **Runs**.
2. Select your run.
3. Go to the **Lineage** tab. You'll see your project's lineage.
4. In the **Lenses** field, select **Latest status**.

## Related docs[​](#related-docs "Direct link to Related docs")

* [About dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md)
* [Set up dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-setup.md)
* [dbt State configs](https://docs.getdbt.com/reference/resource-configs/dbt-state-configs.md)
* [Migrate from state-aware orchestration](https://docs.getdbt.com/docs/deploy/dbt-state-migration.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
