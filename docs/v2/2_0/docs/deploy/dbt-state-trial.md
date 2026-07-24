# dbt State trial and billing [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Login required | Usage-basedⓘ

Start a dbt State trial and manage paid access from the dbt platform **Billing & Usage** dashboard.

## How the trial works[​](#how-the-trial-works "Direct link to How the trial works")

* Eligible new organizations receive 30 days of free use with no usage limit. After the free period, a credit card or enterprise contract (for dbt platform managed plans) is required to continue. For more information, refer to [Continuing after the trial ends](https://docs.getdbt.com/docs/deploy/dbt-state-trial.md#continuing-after-the-trial-ends).
* To start a dbt State trial, you need a dbt account so you can manage dbt State usage, billing, and spend limits from one dashboard. A paid dbt platform plan is *not* required to use dbt State locally.
* Once started, you cannot pause the trial.
* If you're using state-aware orchestration prior to June 1, 2026, your trial is extended until the billing period begins on September 1, 2026. If the extension isn't applied to your account, contact your account team.

## Starting your trial[​](#starting-your-trial "Direct link to Starting your trial")

To start your 30-day trial, refer to the instructions in [Setting up dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-setup.md).

## Continuing after the trial ends [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[​](#continuing-after-the-trial-ends- "Direct link to continuing-after-the-trial-ends-")

Once your trial ends, dbt notifies your billing admin — they must set up paid access to keep using dbt State.

If your account has consumption spend on contract, go to the **State** tab of the **Usage-based features** page and click **Allow** to bill against your committed spend. Otherwise, [contact the dbt Labs sales team](https://www.getdbt.com/contact).

## How billing works[​](#how-billing-works "Direct link to How billing works")

### Daily active target tables[​](#daily-active-target-tables "Direct link to Daily active target tables")

For purposes of pricing, daily active target tables (DATT) are measured as the number of distinct target tables (as defined below) for which dbt State performs at least one of the following unique operations on a given day (based on UTC time): a skip, clone, or test reuse.

A target table is a database object managed by your dbt project for a given database and schema name. It includes seeds, snapshots, dbt models (including incremental models). It also includes each distinct test (even if the tests are not built into the database because `store_failures` is disabled). For example, if `stg_customers` has `not_null` and `unique` tests on its `id` column, that's three target tables: the model and its two tests.

When you run `dbt build` or a similar command, a target table is selected for execution. It counts as an active target table if dbt State can reuse it based on your configuration rules. All reuses of the same active target table in a single day (based on UTC time) count as a single daily active target table (DATT).

### Monthly cost calculation[​](#monthly-cost-calculation "Direct link to Monthly cost calculation")

dbt State calculates cost per billing period using the unit price (USD $0.094) x sum of daily active target tables (DATT) for all account users and all days in that billing period. For example, if you have 100 DATT in a billing period, you'll be billed for 100 \* $0.094 = $9.40.

For current unit price and more information, refer to the [dbt Labs Service Consumption Table](https://www.getdbt.com/legal/service-consumption-table).

### Cancellation[​](#cancellation "Direct link to Cancellation")

Usage is tracked through your cancellation date. You're billed at month end for usage incurred before cancellation and not charged for usage after.

## Setting spend alerts[​](#setting-spend-alerts "Direct link to Setting spend alerts")

You can set a spend alert to get notified when your monthly dbt State costs reach a defined threshold.

1. In your dbt platform account, click your account name in the lower-left corner above your username and click **Account settings**.
2. Go to **Billing & Usage** > **Usage-based features**.
3. In the **Spend alert** section, click **Set a spend alert**.
4. Enable the toggle to receive email notifications when monthly spend reaches your threshold.
5. In the **Alert threshold** field, enter the amount in USD that triggers the alert.
6. Click **Save**.

## Related docs[​](#related-docs "Direct link to Related docs")

* [About dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md)
* [Set up dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-setup.md)
* [dbt State usage and pricing](https://docs.getdbt.com/docs/platform/billing.md#dbt-state-usage)
