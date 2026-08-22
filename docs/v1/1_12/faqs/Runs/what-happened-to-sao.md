# What happened to state-aware orchestration?

On June 1, 2026, dbt Labs and Fivetran announced **[dbt State](../../docs/deploy/dbt-state-about.md)**[Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles") as a new and improved version of state-aware orchestration. A key feature is [`lag_tolerance`](../../reference/resource-configs/lag-tolerance.md), which controls how much time must pass since the last upstream data change before a node is eligible for a rebuild.

dbt State improves upon state-aware orchestration in a few key ways:

* **Works everywhere** — dbt State works with dbt Core, Fusion, and dbt platform, as well as external orchestrators, across both development and deployment environments.
* **Smarter data freshness tracking** — dbt State tracks data freshness across the DAG and automatically propagates it through models materialized as views. Unlike state-aware orchestration's `build_after` config which compares against the model's last successful execution, dbt State's `lag_tolerance` compares against the freshness of the underlying data.
* **Advanced change detection** — dbt State can detect and ignore file modifications that don't change actual transformation logic, such as adding a comment or cleaning up whitespace.

If you were using state-aware orchestration prior to June 1, 2026, you can continue using it. Once you start your free dbt State trial, it will be extended beyond the standard 30-day period. If the extension isn't applied to your account, contact your account team. For details on billing after the trial ends, refer to [dbt State usage and pricing](../../docs/platform/billing.md#dbt-state-usage).

While dbt State is in preview, there is no required migration timeline — dbt Labs will communicate a timeline when dbt State reaches general availability.

To get started, refer to [Migrate from state-aware orchestration](../../docs/deploy/dbt-state-migration.md).
