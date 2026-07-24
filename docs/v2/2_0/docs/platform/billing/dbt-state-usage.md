# dbt State usage

Login required | Usage-basedⓘ

[dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md) enables dbt to reuse nodes by cloning from another location or skipping a rebuild when the logic and data haven't changed. It's a separate, usage-based product available to dbt Core, dbt platform, and dbt Fusion engine users. Learn more about how your usage influences the price so you can plan your savings effectively.

### About free trial[​](#about-free-trial "Direct link to About free trial")

* Eligible new organizations receive 30 days of free use with no usage limit. After the free period, a credit card or enterprise contract (for dbt platform managed plans) is required to continue. For more information, refer to [Continuing after the trial ends](https://docs.getdbt.com/docs/deploy/dbt-state-trial.md#continuing-after-the-trial-ends).
* To start a dbt State trial, you need a dbt account so you can manage dbt State usage, billing, and spend limits from one dashboard. A paid dbt platform plan is *not* required to use dbt State locally.
* Once started, you cannot pause the trial.
* If you're using state-aware orchestration prior to June 1, 2026, your trial is extended until the billing period begins on September 1, 2026. If the extension isn't applied to your account, contact your account team.

### Daily active target tables[​](#daily-active-target-tables "Direct link to Daily active target tables")

For purposes of pricing, daily active target tables (DATT) are measured as the number of distinct target tables (as defined below) for which dbt State performs at least one of the following unique operations on a given day (based on UTC time): a skip, clone, or test reuse.

A target table is a database object managed by your dbt project for a given database and schema name. It includes seeds, snapshots, dbt models (including incremental models). It also includes each distinct test (even if the tests are not built into the database because `store_failures` is disabled). For example, if `stg_customers` has `not_null` and `unique` tests on its `id` column, that's three target tables: the model and its two tests.

When you run `dbt build` or a similar command, a target table is selected for execution. It counts as an active target table if dbt State can reuse it based on your configuration rules. All reuses of the same active target table in a single day (based on UTC time) count as a single daily active target table (DATT).

### Monthly cost calculation[​](#monthly-cost-calculation "Direct link to Monthly cost calculation")

dbt State calculates cost per billing period using the unit price (USD $0.094) x sum of daily active target tables (DATT) for all account users and all days in that billing period. For example, if you have 100 DATT in a billing period, you'll be billed for 100 \* $0.094 = $9.40.

For current unit price and more information, refer to the [dbt Labs Service Consumption Table](https://www.getdbt.com/legal/service-consumption-table).

### Cancellation[​](#cancellation "Direct link to Cancellation")

Usage is tracked through your cancellation date. You're billed at month end for usage incurred before cancellation and not charged for usage after.
