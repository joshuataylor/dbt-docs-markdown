# How does dbt pricing work?

dbt platformⓘ

As a customer, you pay for the number of seats you have and the amount of usage consumed each month. Seats are billed primarily on the amount of Developer and Read licenses purchased.

Usage is based on the number of [Successful Models Built](#what-counts-as-a-successful-model-built) and, if purchased and used, Semantic Layer [Queried Metrics](#what-counts-as-a-queried-metric) subject to reasonable usage. All billing computations are conducted in Coordinated Universal Time (UTC).

### What counts as a seat license?[​](#what-counts-as-a-seat-license "Direct link to What counts as a seat license?")

You can learn more about allocating users to your account in [Users and licenses](https://docs.getdbt.com/docs/platform/manage-access/seats-and-users.md). There are four types of possible seat licenses:

* **Analyst**\* — for permission sets assigned and shared amongst those who don't need day-to-day access.
* **Developer** — for permission sets that require day-to-day interaction with the dbt platform.
* **IT** — for access to specific features related to account management (for example, configuring git integration).
* **Read-Only** — for access to view certain documents and reports.

\* The [Analyst license type](https://docs.getdbt.com/docs/platform/manage-access/about-user-access.md?version=1.12#licenses) is not available for new purchase.

### What counts as a Successful Model Built?[​](#what-counts-as-a-successful-model-built "Direct link to What counts as a Successful Model Built?")

A Successful Model Built is any model successfully built in a dbt deployment environment through dbt’s orchestration. This includes jobs run via the scheduler, CI builds (triggered by pull requests), and runs kicked off via the dbt API. Models that build successfully are counted even if the overall run later fails. For example, if a job containing 100 models fails after 51 are built, only those 51 are counted.

Any models built in a dbt development environment (for example, via the Studio IDE) do not count towards your usage. Tests, seeds, ephemeral models, and snapshots also do not count.

When a dynamic table is initially created, the model is counted (if the creation is successful). However, in subsequent runs, dbt skips these models unless the definition of the dynamic table has changed. This refers not to changes in the SQL logic but to changes in dbt's logic, specifically those governed by [`on_configuration_change config`](https://docs.getdbt.com/reference/resource-configs/on_configuration_change.md)). The dynamic table continues to update on a cadence because the adapter is orchestrating that refresh rather than dbt.

| What counts towards Successful Models Built |    |
| ------------------------------------------- | -- |
| View                                        | ✅ |
| Table                                       | ✅ |
| Incremental                                 | ✅ |
| Ephemeral Models                            | ❌ |
| Tests                                       | ❌ |
| Seeds                                       | ❌ |
| Snapshots                                   | ❌ |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### What counts as a Queried Metric?[​](#what-counts-as-a-queried-metric "Direct link to What counts as a Queried Metric?")

The Semantic Layer, powered by MetricFlow, measures usage in distinct Queried Metrics.

* Every successful request you make to render or run SQL to the Semantic Layer API counts as at least one queried metric, even if no data is returned.
* If the query calculates or renders SQL for multiple metrics, each calculated metric will be counted as a queried metric.
* If a request to run a query is not executed successfully in the data platform or if a query results in an error without completion, it is not counted as a queried metric.
* Requests for metadata from the Semantic Layer are also not counted as queried metrics.

Examples of queried metrics include:

* Querying one metric, grouping by one or more dimensions → 1 queried metric

  ```shell
  dbt sl query --metrics revenue --group-by metric_time
  ```

* Querying two metrics, grouping by two dimensions → 2 queried metrics

  ```shell
  dbt sl query --metrics revenue,gross_sales --group-by metric_time,user__country
  ```

Compiling metrics counts the same way — one queried metric per metric compiled (for example, `dbt sl query --metrics revenue --compile` → 1 queried metric).

### Viewing usage in the product[​](#viewing-usage-in-the-product "Direct link to Viewing usage in the product")

Viewing usage in the product is restricted to specific roles:

* Starter plan — Owner group
* Enterprise and Enterprise+ plans — Account and billing admin roles

If you have access to the **Billing** and **Usage** pages in **Account settings**, you can see an estimate of the month's usage, how your account tracks against it, and which projects are building the most models.

[![To view account-level estimated usage, go to 'Account settings' and then select 'Billing'.](/img/docs/building-a-dbt-project/billing-usage-page.jpg?v=2 "To view account-level estimated usage, go to 'Account settings' and then select 'Billing'.")](#)To view account-level estimated usage, go to 'Account settings' and then select 'Billing'.

As a Starter and Developer plan user, you can see how the account is tracking against the included models built. As an Enterprise plan user, you can see how much you have drawn down from your annual commit and how much remains.

On each **Project home** page, any user with project access can see how many models are built each month, with top jobs by models built available on each **Environment** page.

[![Your Project home page displays how many models are built each month.](/img/docs/building-a-dbt-project/billing-project-page.jpg?v=2 "Your Project home page displays how many models are built each month.")](#)Your Project home page displays how many models are built each month.

The **Job details** page's **Insights** tab shows models built per month for that job and which take longest to build.

[![View how many models are being built per month for a particular job by going to the 'Insights' tab in the 'Job details' page.](/img/docs/building-a-dbt-project/billing-job-page.jpg?v=2 "View how many models are being built per month for a particular job by going to the 'Insights' tab in the 'Job details' page.")](#)View how many models are being built per month for a particular job by going to the 'Insights' tab in the 'Job details' page.

Usage data shown in dbt is only an estimate and may be delayed, and some visualizations aren't available on legacy plans. Your final monthly usage appears on your monthly statements (Starter and Enterprise-tier plans).
