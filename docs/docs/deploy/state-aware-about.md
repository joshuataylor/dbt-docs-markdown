# About state-aware orchestration [Private preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

Every time a job runs, state-aware orchestration automatically determines which models to build by detecting changes in code or data.

<!-- -->

important

The dbt Fusion engine is currently available for installation in:

* [Local command line interface (CLI) tools](https://docs.getdbt.com/docs/fusion/install-fusion-cli.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [VS Code and Cursor with the dbt extension](https://docs.getdbt.com/docs/install-dbt-extension.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [dbt platform environments](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-version-in-cloud.md#dbt-fusion-engine) [Private preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Join the conversation in our Community Slack channel [`#dbt-fusion-engine`](https://getdbt.slack.com/archives/C088YCAB6GH).

Read the [Fusion Diaries](https://github.com/dbt-labs/dbt-fusion/discussions/categories/announcements) for the latest updates.

State-aware orchestration saves you compute costs and reduces runtime because when a job runs, it checks for new records and only builds the models that will change.

[![Fusion powered state-aware orchestration](/img/docs/deploy/sao.gif?v=2 "Fusion powered state-aware orchestration")](#)Fusion powered state-aware orchestration

We built dbt's state-aware orchestration on these four core principles:

* **Real-time shared state:** All jobs write to a real-time shared model-level state, allowing dbt to rebuild only changed models regardless of which jobs the model is built in.
* **Model-level queueing:** Jobs queue up at the model-level so you can avoid any 'collisions' and prevent rebuilding models that were just updated by another job.
* **State-aware and state agnostic support:** You can build jobs dynamically (state-aware) or explicitly (state-agnostic). Both approaches update shared state so everything is kept in sync.
* **Sensible defaults:** State-aware orchestration works out-of-the-box (natively), with an optional configuration setting for more advanced controls. For more information, refer to [state-aware advanced configurations](https://docs.getdbt.com/docs/deploy/state-aware-setup.md#advanced-configurations).

note

State-aware orchestration does not depend on [static analysis](https://docs.getdbt.com/docs/fusion/new-concepts.md#principles-of-static-analysis) and works even when `static_analysis` is disabled.

## Optimizing builds with state-aware orchestration[​](#optimizing-builds-with-state-aware-orchestration "Direct link to Optimizing builds with state-aware orchestration")

State-aware orchestration uses shared state tracking to determine which models need to be built by detecting changes in code or data every time a job runs. It also supports custom refresh intervals and custom source freshness configurations, so dbt only rebuilds models when they're actually needed.

For example, you can configure your project so that dbt skips rebuilding the `dim_wizards` model (and its parents) if they’ve already been refreshed within the last 4 hours, even if the job itself runs more frequently.

Without configuring anything, dbt's state-aware orchestration automatically knows to build your models either when the code has changed or if there’s any new data in a source (or upstream model in the case of [dbt Mesh](https://docs.getdbt.com/docs/mesh/about-mesh.md)).

**Note:** When a model fails a [data test](https://docs.getdbt.com/docs/build/data-tests.md), state-aware orchestration rebuilds it on subsequent runs instead of reusing it from prior state. This ensures dbt reevaluates models with unresolved data quality issues.

### Handling concurrent jobs[​](#handling-concurrent-jobs "Direct link to Handling concurrent jobs")

If two separate jobs both depend on the same downstream model (for example, `model_ab`) and both detect upstream changes (`updates_on = any`), `model_ab` could run twice — once for each job. However, if `model_ab` was already built and nothing has changed since that build, neither job will rebuild it. Instead, both jobs will reuse the existing version instead of rebuilding.

Under state-aware orchestration, all jobs read and write from the same shared state and build a model only when either the code or data state has changed. This means that each job individually evaulates whether a model needs rebuilding based on the model’s compiled code and upstream data state.

What happens when jobs overlap:

* If both jobs reach the same model at exactly the same time, one job waits until the other finishes. This is to prevent collisions in the data warehouse when two jobs try to build the same model at the same time.
* After the first job finishes building the model, the second job still checks whether a rebuild is needed. If there are new data or code changes to incorporate, the second job builds the model again. If there are no changes and building the model would produce the same result, the second job reuses the model.

To prevent a job from being built too frequently even when the code or data state has changed, you can reduce build frequency by using the `build_after` config. For information on how to use `build_after`, refer to [Model freshness](https://docs.getdbt.com/reference/resource-configs/freshness.md) and [Advanced configurations](https://docs.getdbt.com/docs/deploy/state-aware-setup.md#advanced-configurations).

### Handling deleted tables[​](#handling-deleted-tables "Direct link to Handling deleted tables")

State-aware orchestration detects and rebuilds models when their tables are deleted in the warehouse, even if there are no code or data changes.

When a table is deleted in the warehouse:

* dbt raises a warning that the expected table is missing.
* The affected model is queued for rebuild during the current run, even if there are no code or data changes.

This behavior ensures consistency between the dbt state and the actual warehouse state. It also reduces the need to manually clear cache or disable state-aware orchestration when models are modified outside of dbt.

## Efficient testing in state-aware orchestration [Private beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[​](#efficient-testing-in-state-aware-orchestration- "Direct link to efficient-testing-in-state-aware-orchestration-")

Private beta feature

State-aware orchestration features in the dbt platform are only available in Fusion, which is in private preview. Contact your account manager to enable Fusion in your account.

Data quality can get degraded in two ways:

* New code changes definitions or introduces edge cases.
* New data, like duplicates or unexpected values, invalidates downstream metrics.

Running dbt’s out-of-the-box [data tests](https://docs.getdbt.com/docs/build/data-tests.md) (`unique`, `not_null`, `accepted_values`, `relationships`) on every build helps catch data errors before they impact business decisions. Catching these errors often requires having multiple tests on every model and running tests even when not necessary. If nothing relevant has changed, repeated test executions don’t improve coverage and only increase cost.

With Fusion, dbt gains an understanding of the SQL code based on the logical plan for the compiled code. dbt then can determine when a test must run again, or when a prior upstream test result can be reused.

Efficient testing in state-aware orchestration reduces warehouse costs by avoiding redundant data tests and combining multiple tests into one run. This feature includes two optimizations:

* **Test reuse** — Tests are reused in cases where no logic in the code or no new data could have changed the test's outcome.
* **Test aggregation** — When there are multiple tests on a model, dbt combines tests to run as a single query against the warehouse, rather than running separate queries for each test.

Currently, Efficient testing is only available in deploy jobs, not in continuous integration (CI) or merge jobs.

### Supported data tests[​](#supported-data-tests "Direct link to Supported data tests")

The following tests can be reused when Efficient testing is enabled:

* [`unique`](https://docs.getdbt.com/reference/resource-properties/data-tests.md#unique)
* [`not_null`](https://docs.getdbt.com/reference/resource-properties/data-tests.md#not_null)
* [`accepted_values`](https://docs.getdbt.com/reference/resource-properties/data-tests.md#accepted_values)

### Enabling Efficient testing[​](#enabling-efficient-testing "Direct link to Enabling Efficient testing")

Before enabling Efficient testing, make sure you have configured [`static_analysis`](https://docs.getdbt.com/docs/fusion/new-concepts.md#configuring-static_analysis).

To enable Efficient testing:

1. From the main menu, go to **Orchestration** > **Jobs**.
2. Select your deploy job. Go to your job settings and click **Edit**.
3. Under **Enable Fusion cost optimization features**, expand **More options**.
4. Select **Efficient testing**. This feature is disabled by default.
5. Click **Save**.

### Example[​](#example "Direct link to Example")

In the following query, you’re joining an `orders` and a `customers` table:

```sql
with

orders as (

    select * from {{ ref('orders') }}

),

customers as (

    select * from {{ ref('customers') }}

),

joined as (

    select
        customers.customer_id as customer_id,
        orders.order_id as order_id
    from customers
    left join orders
        on orders.customer_id = customers.customer_id

)

select * from joined
```

* `not_null` test: A `left join` can introduce null values for customers without orders. Even if upstream tests verified `not_null(order_id)` in orders, the join can create null values downstream. dbt must always run a `not_null` test on `order_id` in this joined result.

* `unique` test: If `orders.order_id` and `customers.customer_id` are unique upstream, uniqueness of `order_id` is preserved and the upstream result can be reused.

### Limitations[​](#limitations "Direct link to Limitations")

The following section lists some considerations when using Efficient testing in state-aware-orchestration:

* **Aggregated tests do not support custom configs**. Tests that include the following [custom config options](https://docs.getdbt.com/reference/data-test-configs.md) will run individually rather than as part of the aggregated batch:

  ```yaml
  config:
    fail_calc: <string>
    limit: <integer>
    severity: error | warn
    error_if: <string>
    warn_if: <string>
    store_failures: true | false
    where: <string>
  ```

* **Efficient testing is available only in deploy jobs**. CI and merge jobs currently do not have the option to enable this feature.

## Related FAQs[​](#related-faqs "Direct link to Related FAQs")

How is state-aware orchestration different from using selectors in dbt Core?

In dbt Core, running with the selectors `state:modified+` and `source_status:fresher+` builds models that either:

* Have changed since the prior run (`state:modified+`)
* Have upstream sources that are fresher than in the prior run (`source_status:fresher+`)

Instead of relying only on these selectors and prior-run artifacts, state-aware orchestration decides whether to rebuild a model based on:

* Compiled SQL diffs that ignore non-meaningful changes like whitespace and comments
* Upstream data changes at runtime and model-level freshness settings
* Shared state across jobs

While dbt Core uses selectors like `state:modified+` and `source_status:fresher+` to decide what to build *only for a single run in a single job*, state-aware orchestration with Fusion maintains a *shared, real-time model state across every job in the environment* and uses that state to determine whether a model’s code or upstream data have actually changed before rebuilding. This ensures dbt only rebuilds models when something has changed, no matter which job runs them.

## Related docs[​](#related-docs "Direct link to Related docs")

* [State-aware orchestration configuration](https://docs.getdbt.com/docs/deploy/state-aware-setup.md)
* [Artifacts](https://docs.getdbt.com/docs/deploy/artifacts.md)
* [Continuous integration (CI) jobs](https://docs.getdbt.com/docs/deploy/ci-jobs.md)
* [`freshness`](https://docs.getdbt.com/reference/resource-configs/freshness.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
