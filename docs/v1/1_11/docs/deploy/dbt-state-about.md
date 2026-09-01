# About dbt State [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Login required | Usage-based

dbt State makes dbt smarter about what to build. Instead of rebuilding every node on every run, dbt reuses nodes by cloning from another location or skipping a rebuild when the logic and data haven't changed.

With dbt State, dbt first compares the logic and data of each node to previous builds across multiple environments on every run — whether orchestrated in the dbt platform, through your own orchestrator, or in development. If the logic is the same and the data is still fresh, dbt reuses an existing object. It will either clone an existing node from elsewhere, or skip executing a model that already exists, rather than building it anew. Additionally, it will automatically defer to production state without the need to manually set the `--defer` or `--state` flags.

dbt State can reuse all node types that create relations in the database (such as models, snapshots, seeds) and data tests.

dbt State works with dbt (v1 and v2) and the dbt platform, across all environments and orchestrators, making it a flexible approach regardless of how you run dbt. It requires authentication through a dbt platform account. For pricing details, refer to [dbt State usage and pricing](../platform/billing/dbt-state-usage.md).

## Benefits

dbt State delivers efficiency gains across both production and development environments:

* **Fresher data, lower costs**: Nodes only rebuild when the result would be different (new data or code changes), reducing warehouse compute while keeping production data fresh.
* **Faster iteration cycles**: In development, dbt automatically clones selected nodes from production whenever possible, so you spend less time waiting for builds and more time writing code.
* **Smarter than standard deferral**: Unlike standard deferral, which always builds selected nodes and only defers unselected upstream references, dbt State decides whether transformations need to run at all, or whether an existing table can simply be cloned.
* **Model-level freshness threshold**: The [`lag_tolerance`](../../reference/resource-configs/lag-tolerance.md) config sets how much time must pass since the last upstream data change before dbt triggers a rebuild. It decouples downstream models from high-frequency upstream changes, and prevents costly rebuilds on stagnant data when an upstream dependency misses its freshness [Service Level Agreement (SLA)](https://www.getdbt.com/blog/data-slas-best-practices).

## How dbt State works

When you run a command like `dbt build --select +my_model`, dbt State evaluates each selected node and applies the most efficient approach it can:

* **Reuse node from same schema (skip)** — dbt checks whether the object already exists in the target schema, its logic hasn't changed, and its upstream parents haven't received fresh data beyond the configured [`lag_tolerance`](../../reference/resource-configs/lag-tolerance.md). If all conditions are met, dbt skips the node entirely, as if it was never selected. For data tests, if the nodes being tested haven't changed since the last run, the previous test result is reused without re-executing the test query.

  For views, if the view's logic is unchanged, dbt State reuses it even if new data has arrived upstream. Because views don't store data, new upstream data is automatically reflected when the view is queried, even without a rebuild. Note that views using `select *` on an upstream node may behave differently — refer to [Views with `select *`](../../faqs/State/views-rebuilt.md#views-with-select) for more information.

* **Reuse node from different schema (clone)** — dbt State looks across all environments and jobs for a matching object with identical logic and fresh data. This includes schemas where a model was built before it ever ran in production. When multiple candidates exist, dbt State clones from the one with the freshest data, regardless of which environment it came from. For example, if a CI schema has fresher data than production and identical logic, dbt State clones from there. The node is marked as **Reused** at a fraction of the compute cost.

  If you want to prevent cloning into a specific target (for example, in regulated environments), set [`allow_clones: false`](../../reference/resource-configs/allow-clones.md) on that target in `profiles.yml` or as an [extended attribute](../dbt-platform-environments.md#extended-attributes) in the dbt platform.

* **Normal build** — If reuse is not possible, dbt builds the node as normal, automatically deferring any unselected upstream nodes.

dbt State fetches table metadata (for example, last-modified timestamps) in the background at the start of each run. Any node ready to skip, clone, or execute proceeds immediately; nodes with an undetermined action wait for the fetch to complete.

Without dbt State, every selected node rebuilds on every run regardless of whether anything has changed.

To see which decision dbt State made for each node after a run and why, you can run the (Applies to dbt v1.99 and earlier) [`dbt-state explain`](../../reference/commands/state-explain.md) command.

For the full list of available configs, see [dbt State configs](../../reference/resource-configs/dbt-state-configs.md).

 How dbt State decides whether to rebuild, clone, or reuse

The following decision tree shows how dbt State chooses the most efficient valid action for each node.

![Decision tree showing how dbt State chooses whether to rebuild, clone, or reuse a node based on state bypasses, volatile SQL handling, execution hashes, freshness, schema matches, clone eligibility, and whether fresh upstream data can still be cloned from time travel or another schema](/img/docs/deploy/run-cache-decision-tree.png?v=2 "dbt State decision tree for rebuild, clone, and reuse")dbt State decision tree for rebuild, clone, and reuse

The key idea is that dbt State only skips work when it can prove the existing object is sufficiently equivalent for the current run. If the SQL logic, relevant config, schema, or upstream freshness means the result might be different, dbt rebuilds instead.

## Signing up for dbt State

dbt State is connected to your existing dbt platform account. Your dbt State credentials are the same as your platform credentials, and dbt State has access to your platform environments and jobs.

dbt State app retirement

The standalone dbt State app (`app.state.dbt.com`) is being retired and is no longer accepting new users. If you have an existing dbt State app account, [create a free dbt platform account](http://us1.dbt.com/register?_dbtsrc=dbt-state) to continue using dbt State — your free trial will be extended upon account creation.

## FAQs

What happened to state-aware orchestration?

On June 1, 2026, dbt Labs and Fivetran announced **[dbt State](./dbt-state-about.md)**[Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles") as a new and improved version of state-aware orchestration. A key feature is [`lag_tolerance`](../../reference/resource-configs/lag-tolerance.md), which controls how much time must pass since the last upstream data change before a node is eligible for a rebuild.

dbt State improves upon state-aware orchestration in a few key ways:

* **Works everywhere** — dbt State works with dbt Core, Fusion, and dbt platform, as well as external orchestrators, across both development and deployment environments.
* **Smarter data freshness tracking** — dbt State tracks data freshness across the DAG and automatically propagates it through models materialized as views. Unlike state-aware orchestration's `build_after` config which compares against the model's last successful execution, dbt State's `lag_tolerance` compares against the freshness of the underlying data.
* **Advanced change detection** — dbt State can detect and ignore file modifications that don't change actual transformation logic, such as adding a comment or cleaning up whitespace.

If you were using state-aware orchestration prior to June 1, 2026, you can continue using it. Once you start your free dbt State trial, it will be extended beyond the standard 30-day period. If the extension isn't applied to your account, contact your account team. For details on billing after the trial ends, refer to [dbt State usage and pricing](../platform/billing.md#dbt-state-usage).

While dbt State is in preview, there is no required migration timeline — dbt Labs will communicate a timeline when dbt State reaches general availability.

To get started, refer to [Migrate from state-aware orchestration](./dbt-state-migration.md).

How is dbt State different from using state:modified?

`state:modified` in dbt Core requires manual management of `manifest.json`, which is cumbersome and error-prone. dbt State is completely managed with almost zero setup and no workflow changes.

`state:modified` only checks if a file has changed. dbt State has semantic understanding of SQL, so meaningless changes like whitespace or table aliases are not counted as a change — making dbt State smarter about what actually needs to rebuild.

`state:modified` does not consider upstream data changes. dbt State checks all sources to see if there is any new data or if the schema has been modified. This enables dbt State to skip running models if the result of the run would be the same as before. For example, if you run `dbt run` with `state:modified` twice, it runs all modified models both times. dbt State only reruns models the second time if upstream sources have changed.

dbt State also has the ability to auto-defer refs and automatically clone tables when the result of the clone would have been the same as a full model run.

`state:modified` has a limitation on seed files over 1MB, while dbt State does not.

Does dbt State support incremental models?

dbt State works with incremental models. When you make a change to an incremental model and run it in development, dbt State automatically clones the model from production if it exists, then runs the new model logic on top of the clone.

If you want to revert to the original dbt behavior and fully refresh the incremental model, pass the [`--full-refresh` flag](../../reference/commands/run.md#refresh-incremental-models).

How is data stored in dbt State?

dbt State sends the following metadata to dbt Labs servers:

* **Last-modified timestamps**: Used to determine whether upstream data has changed since the last run
* **SQL statement hashes**: SQL statements are processed to detect and classify changes, then hashed. Only the hash is persisted for future comparisons.

No actual data from your warehouse is transmitted.

The dbt State service runs in a single US multi-tenant (MT) instance. The service never connects to your data warehouse. No actual data from your warehouse is transmitted. The only connection is to your running dbt process (CLI or platform) in order to exchange the metadata described above. For data retention details, refer to the [dbt Labs privacy policy](https://www.getdbt.com/cloud/privacy-policy).

Where does the metadata about last updated timestamp come from?

Last updated timestamps come directly from the data warehouse, for example from `INFORMATION_SCHEMA` tables.

How does dbt State calculate that a model has changed?

dbt State only considers substantial changes to a model. Because dbt State understands the entire lineage of your models, it can see through things like whitespace and aliases to determine whether a model is the same or different across environments.

By default, dbt State compares rendered SQL to detect code changes. Any change to the rendered SQL — including from non-deterministic macros or environment variables — triggers a rebuild.

You can enable [`compare_unrendered_code`](../../reference/resource-configs/compare-unrendered-code.md) to also check the Jinja template (unrendered code). When enabled, a rebuild only occurs when both the template *and* the rendered SQL have changed. Non-deterministic values (for example, `{{ env_var('AIRFLOW_RUN_ID') }}` or a macro that calls `uuid()`) don't trigger rebuilds as long as the template itself is unchanged, which helps avoid unnecessary warehouse compute costs.

Why is my model being rebuilt instead of reused?

dbt State decides whether to reuse a model by parsing the rendered SQL into a syntax tree and comparing the hash. If the hash has changed (implying the model's logic has changed), dbt State rebuilds the model.

dbt State prioritizes safety and precision; if it can't guarantee skipping a node is safe, then it rebuilds the node to be sure. A few patterns that commonly cause overeager rebuilds are listed on this page, along with recommendations to increase reuse rate.

The following patterns commonly cause unexpected rebuilds:

* [Views with `select *`](#views-with-select)
* [Non-deterministic Jinja templating](#non-deterministic-jinja-templating)
* [Models with external sources in BigQuery](#models-with-external-sources-in-bigquery)

## Views with `select *`

dbt State reuses a model when its compiled SQL matches the stored hash. When a view uses `select *` directly on a `ref()` or `source()`, dbt can't determine the column list at parse time — the upstream model or source table might have gained or lost columns since the last run. To be safe, dbt State forces a rebuild.

For example, this view will be rebuilt even if `stg_orders` hasn't changed because dbt can't know at parse time whether `stg_orders` has the same columns as before:

```sql
-- stg_orders_view.sql (materialized: view)
select * from {{ ref('stg_orders') }}
```

However, if you use `select *` on a CTE, dbt can resolve the columns from the CTE definition and safely reuse the view:

```sql
with renamed as (
    select order_id, customer_id, order_total from {{ ref('stg_orders') }}
)

select * from renamed
```

If a CTE explicitly names its columns, a `select *` that reads from that CTE won't force a rebuild even if an earlier CTE used `select *` on a `ref()` or `source()`. The typical staging pattern is reused:

```sql
with source as (
    select * from {{ source('jaffle_shop', 'orders') }}
),

renamed as (
    select
        id as order_id,
        user_id as customer_id,
        amount as order_total
    from source
)

select * from renamed
```

tip

To avoid forced rebuilds, use explicit column names when selecting directly from a `ref()` or `source()`. You can also exclude views from execution using `--exclude config.materialized:view`.

## Non-deterministic Jinja templating

Some macros and environment variables can cause unexpected rebuilds. For example, `dbt_utils.get_relations_by_pattern` (an introspective macro) combined with `dbt_utils.union_relations` can return relations in a different order on each run, producing different rendered SQL even when your project logic hasn't changed. Similarly, environment variables that change between runs produce different rendered SQL on every run:

```sql
select '{{ env_var("AIRFLOW_RUN_ID") }}' as airflow_run_id, ...
```

Because the query result order or the environment variable's value changes, the rendered SQL differs from the stored hash on every run. dbt State treats this as a code change and rebuilds the model, even though the underlying project logic hasn't changed. This pattern can affect any model type, not just views; if a base or staging model rebuilds on every run, all of its downstream models rebuild, too.

To avoid these unnecessary rebuilds, enable [`compare_unrendered_code`](../../reference/resource-configs/compare-unrendered-code.md). When enabled, dbt State checks both the Jinja template and rendered SQL; non-deterministic values that don't change the template don't trigger a rebuild. For example:

```sql
{{ config(state={"compare_unrendered_code": true}) }}

select '{{ env_var("AIRFLOW_RUN_ID") }}' as airflow_run_id, ...
```

## Models with external sources on BigQuery

On BigQuery, models that use external sources (such as Google Sheets) always rebuild because BigQuery doesn't expose modification timestamps for external sources, so dbt State can't determine freshness.

tip

To prevent external sources from always being considered stale, configure [`loaded_at_field`](../../reference/resource-properties/freshness.md#loaded_at_field) or [`loaded_at_query`](../../reference/resource-properties/freshness.md#loaded_at_query) in your source definition to point to a timestamp field. This lets dbt State query a timestamp field directly to determine freshness, rather than relying on warehouse metadata.

## How to diagnose

After a run, use (Applies to dbt v1.99 and earlier) [`dbt-state explain`](../../reference/commands/state-explain.md) to see why dbt State rebuilt, reused, or cloned a specific model. For a detailed breakdown, use the `--verbose` flag with `-s` to select your model:

note

The command name differs by version: dbt Core 2.0 uses `dbt state explain` (with a space), while dbt Core v1.x uses `dbt-state explain` (with a hyphen).

(Applies to dbt v1.99 and earlier)

```bash
dbt-state explain --verbose -s my_model_name
```

What happens if dbt State servers fail?

If dbt State servers are unavailable, dbt gracefully falls back to normal dbt behavior.

What if I work on multiple projects that each use their own dbt State?

You can specify your org ID in `dbt_project.yml`:

```yaml
dbt-cloud:
  state-org-id: <your-org-id>
```

What if my prod environment isn't named prod?

You can specify the defer-to environment using the [`defer_to_target`](../../reference/resource-configs/defer-to-target.md) config in `profiles.yml`:

```yaml
my_project:
  outputs:
    prod:
      type: snowflake
      defer_to_target: production
```

`defer_to_target` only applies to self-managed deployments. If you're using the dbt platform, deferral is configured through your environment settings in the UI. For more details, refer to [Configuring deferral](./dbt-state-deferral.md).

## Related docs

* [Set up dbt State](./dbt-state-setup.md)
* [Non-interactive environment setup](./dbt-state-cicd.md)
* [dbt State configs](../../reference/resource-configs/dbt-state-configs.md)
* [Migrate from state-aware orchestration](./dbt-state-migration.md)
* [dbt State trial and billing](./dbt-state-trial.md)
* [dbt State usage and pricing](../platform/billing/dbt-state-usage.md)
