# About dbt State [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

dbt State makes dbt smarter about what to build. Instead of rebuilding every node on every run, dbt reuses nodes by cloning from another location or skipping a rebuild when the logic and data haven't changed.

With dbt State, dbt first compares the logic and data of each node to previous builds across multiple environments on every run — whether orchestrated in the dbt platform, through your own orchestrator, or in development. If the logic is the same and the data is still fresh, dbt reuses an existing object. It will either clone an existing node from elsewhere, or skip executing a model that already exists, rather than building it anew. Additionally, it will automatically defer to production state without the need to manually set the `--defer` or `--state` flags.

dbt State can reuse all node types that create relations in the database (such as models, snapshots, seeds) and data tests.

dbt State works with dbt Core, the dbt platform, and dbt Fusion engine, across all environments and orchestrators, making it a flexible approach regardless of how you run dbt. It requires authentication either through a dbt platform account or a [standalone dbt State account](https://app.state.dbt.com). For pricing details, refer to [dbt State usage and pricing](https://docs.getdbt.com/docs/platform/billing.md#dbt-state-usage).

## Benefits[​](#benefits "Direct link to Benefits")

dbt State delivers efficiency gains across both production and development environments:

* **Fresher data, lower costs** — Nodes only rebuild when the result would be different (new data or code changes), reducing warehouse compute while keeping production data fresh.
* **Faster iteration cycles** — In development, dbt automatically clones selected nodes from production whenever possible, so you spend less time waiting for builds and more time writing code.
* **Smarter than standard deferral** — Unlike standard deferral, which always builds selected nodes and only defers unselected upstream references, dbt State decides whether transformations need to run at all, or whether an existing table can simply be cloned.

## How dbt State works[​](#how-dbt-state-works "Direct link to How dbt State works")

When you run a command like `dbt build --select +my_model`, dbt State evaluates each selected node and applies the most efficient approach it can:

* **Reuse node from same schema (skip)** — dbt checks whether the object already exists in the target schema, its logic hasn't changed, and its upstream parents haven't received fresh data beyond the configured [`lag_tolerance`](https://docs.getdbt.com/reference/resource-configs/lag-tolerance.md). If all conditions are met, dbt skips the node entirely, as if it was never selected. For data tests, if the nodes being tested haven't changed since the last run, the previous test result is reused without re-executing the test query.
* **Reuse node from different schema (clone)** — If the same object exists with matching logic and fresh data, dbt State clones it. The node is marked as **Reused** at a fraction of the compute cost.
* **Normal build** — If reuse is not possible, dbt builds the node as normal, automatically deferring any unselected upstream nodes.

Without dbt State, every selected node rebuilds on every run regardless of whether anything has changed.

For the full list of available configs, see [dbt State configs](https://docs.getdbt.com/reference/resource-configs/dbt-state-configs.md).

## Supported data warehouses[​](#supported-data-warehouses "Direct link to Supported data warehouses")

dbt State is supported on the following data warehouses:

* Snowflake
* Databricks
* BigQuery
* Redshift

More data warehouses are on the roadmap. If you're using another data warehouse and are interested in dbt State, [let us know](https://www.getdbt.com/contact).

## Signing up for dbt State[​](#signing-up-for-dbt-state "Direct link to Signing up for dbt State")

When you sign up for dbt State, you'll choose one of two paths:

* **dbt platform account** — dbt State is connected to your existing dbt platform account. Your dbt State credentials are the same as your platform credentials, and dbt State has access to your platform environments and jobs.
* **Standalone account ([app.state.dbt.com](https://app.state.dbt.com))** — A standalone dbt State account is independent of any dbt platform account. You manage dbt State credentials separately, and dbt State has no visibility into your platform environments or jobs.

A standalone account makes sense if you:

* Don't have a dbt platform account
* Don't have admin permissions to enable dbt State in your dbt platform account
* Want to test dbt State without connecting it to your dbt platform account yet

## FAQs[​](#faqs "Direct link to FAQs")

What happened to state-aware orchestration?

On June 1, 2026, dbt Labs and Fivetran announced **[dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md)**[Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles") as a new and improved version of state-aware orchestration. A key feature is [`lag_tolerance`](https://docs.getdbt.com/reference/resource-configs/lag-tolerance.md), which controls how much time must pass since the last upstream data change before a node is eligible for a rebuild.

dbt State improves upon state-aware orchestration in a few key ways:

* **Works everywhere** — dbt State works with dbt Core, Fusion, and dbt platform, as well as external orchestrators, across both development and deployment environments.
* **Smarter data freshness tracking** — dbt State tracks data freshness across the DAG and automatically propagates it through models materialized as views. Unlike state-aware orchestration's `build_after` config which compares against the model's last successful execution, dbt State's `lag_tolerance` compares against the freshness of the underlying data.
* **Advanced change detection** — dbt State can detect and ignore file modifications that don't change actual transformation logic, such as adding a comment or cleaning up whitespace.

If you were using state-aware orchestration prior to June 1, 2026, you can continue using it. Your dbt State trial will be extended until the billing period begins on September 1, 2026. If your trial wasn't extended, contact your account team. For details on billing after the trial ends, refer to [dbt State usage and pricing](https://docs.getdbt.com/docs/platform/billing.md#dbt-state-usage).

While dbt State is in preview, there is no required migration timeline — dbt Labs will communicate a timeline when dbt State reaches general availability.

To get started, refer to [Migrate from state-aware orchestration](https://docs.getdbt.com/docs/deploy/dbt-state-migration.md).

How is dbt State different from using state:modified?

`state:modified` in dbt Core requires manual management of `manifest.json`, which is cumbersome and error-prone. dbt State is completely managed with almost zero setup and no workflow changes.

`state:modified` only checks if a file has changed. dbt State has semantic understanding of SQL, so meaningless changes like whitespace or table aliases are not counted as a change — making dbt State smarter about what actually needs to rebuild.

`state:modified` does not consider upstream data changes. dbt State checks all sources to see if there is any new data or if the schema has been modified. This enables dbt State to skip running models if the result of the run would be the same as before. For example, if you run `dbt run` with `state:modified` twice, it runs all modified models both times. dbt State only reruns models the second time if upstream sources have changed.

dbt State also has the ability to auto-defer refs and automatically clone tables when the result of the clone would have been the same as a full model run.

`state:modified` has a limitation on seed files over 1MB, while dbt State does not.

Does dbt State support incremental models?

dbt State works with incremental models. When you make a change to an incremental model and run it in development, dbt State automatically clones the model from production if it exists, then runs the new model logic on top of the clone.

If you want to revert to the original dbt behavior and fully refresh the incremental model, pass the [`--full-refresh` flag](https://docs.getdbt.com/reference/commands/run.md#refresh-incremental-models).

How is data stored in dbt State?

dbt State sends last-modified timestamps and SQL statements to dbt Labs servers. SQL statements are hashed and then discarded, so dbt Labs cannot see the contents after hashing. These hashes are used to identify whether a statement has changed by comparing them on each run.

Where does the metadata about last updated timestamp come from?

Last updated timestamps come directly from the data warehouse, for example from `INFORMATION_SCHEMA` tables.

How does dbt State calculate that a model has changed?

dbt State only considers substantial changes to a model. Because dbt State understands the entire lineage of your models, it can see through things like whitespace and aliases to determine whether a model is the same or different across environments.

Why is my model being rebuilt instead of reused?

dbt State decides whether to reuse a model by comparing its compiled SQL to the hash from the previous run. If anything changes that compiled SQL between runs, even when the logic hasn't meaningfully changed, dbt State rebuilds the model.

These two patterns commonly cause unexpected rebuilds:

* [Views with `select *`](#views-with-select-)
* [Non-deterministic Jinja templating](#non-deterministic-jinja-templating)

## Views with `select *`[​](#views-with-select- "Direct link to views-with-select-")

dbt State always rebuilds views that use `select *` anywhere in their SQL, including inside CTEs. A common staging pattern like the following triggers this behavior:

```sql
with source as (
    select * from {{ source("my_source", "my_table") }}
),

renamed as (
    select
        id as order_id,
        ...
    from source
)

select * from renamed
```

dbt State reuses a model when its compiled SQL matches the stored hash. For views with `select *`, dbt State can't determine which columns the query selects without querying the upstream schema, so it can't confirm the SQL is unchanged. It always rebuilds these views to avoid reusing a stale result. When dbt State rebuilds a view, it also re-runs any tests defined on the model.

tip

To make this view eligible for reuse, remove the imported CTE and reference the source directly with explicit column names:

```sql
select
    id as order_id,
    ...
from {{ source("my_source", "my_table") }}
```

If you can't remove `select *`, you can exclude views from running with `--exclude config.materialized:view`.

## Non-deterministic Jinja templating[​](#non-deterministic-jinja-templating "Direct link to Non-deterministic Jinja templating")

Some macros, such as `dbt_utils.get_relations_by_pattern` (an introspective macro) combined with `dbt_utils.union_relations`, can return relations in a different order on each run. That produces different compiled SQL even when your project logic hasn't changed. dbt State detects a new hash and rebuilds the model.

This pattern can affect any model type, not just views. If a base or staging model rebuilds on every run, all of its downstream models rebuild, too.

## How to diagnose[​](#how-to-diagnose "Direct link to How to diagnose")

In dbt Core v1.7–v1.12, run the `dbt-state explain` command to see why dbt State rebuilt or reused a specific model.

Experimental

The `dbt-state explain` command is experimental. It isn't available in the dbt Fusion engine or dbt platform yet.

What happens if dbt State servers fail?

If dbt State servers are unavailable, dbt gracefully falls back to normal dbt behavior.

What if I work on multiple projects that each use their own dbt State?

You can specify your org ID in `dbt_project.yml`:

```yaml
dbt-cloud:
  state-org-id: <your-org-id>
```

What if my prod environment isn't named prod?

You can specify the defer-to environment using the [`defer_to_target`](https://docs.getdbt.com/reference/resource-configs/defer-to-target.md) config in `profiles.yml`:

```yaml
my_project:
  outputs:
    prod:
      type: snowflake
      defer_to_target: production
```

`defer_to_target` only applies to self-managed deployments. If you're using the dbt platform, deferral is configured through your environment settings in the UI. For more details, refer to [Configuring deferral](https://docs.getdbt.com/docs/deploy/dbt-state-deferral.md).

## Related docs[​](#related-docs "Direct link to Related docs")

* [Set up dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-setup.md)
* [Non-interactive environment setup](https://docs.getdbt.com/docs/deploy/dbt-state-cicd.md)
* [dbt State configs](https://docs.getdbt.com/reference/resource-configs/dbt-state-configs.md)
* [Migrate from state-aware orchestration](https://docs.getdbt.com/docs/deploy/dbt-state-migration.md)
* [dbt State usage and pricing](https://docs.getdbt.com/docs/platform/billing.md#dbt-state-usage)
