# dbt State trial and billing

Login required | Usage-based

Start a dbt State trial, continue on your plan's free monthly DATT allowance, or manage paid access from the dbt platform **Billing & Usage** dashboard.

## How the trial works

* Eligible new organizations receive 30 days of free use with no usage limit.
* To start a dbt State trial, you need a dbt account so you can manage dbt State usage, billing, and spend limits from one dashboard. A paid dbt platform plan is *not* required to use dbt State locally. To start your trial, refer to the instructions in [Setting up dbt State](./dbt-state-setup.md).
* Once started, you cannot pause the trial.
* Seven days before your trial ends, your admin gets an email with what's coming: the trial end date, the monthly DATT allowance your account will receive, and how to upgrade if needed.
* The [free monthly DATT allowance](#free-monthly-datt-allowance) includes a set amount of daily active target tables for your account. The allowance amount and what happens when you reach the limit depend on your plan.
* If you were using state-aware orchestration prior to June 1, 2026, your dbt State trial will be extended beyond the standard 30-day period. If the extension isn't applied to your account, contact your account team.

## How billing works

### Daily active target tables

For purposes of pricing, daily active target tables (DATT) are measured as the number of distinct target tables (as defined below) for which dbt State performs at least one of the following unique operations on a given day (based on UTC time): a skip, clone, or test reuse.

A target table is a database object managed by your dbt project for a given database and schema name. It includes seeds, snapshots, dbt models (including incremental models). It also includes each distinct test (even if the tests are not built into the database because `store_failures` is disabled). For example, if `stg_customers` has `not_null` and `unique` tests on its `id` column, that's three target tables: the model and its two tests.

When you run `dbt build` or a similar command, a target table is selected for execution. It counts as an active target table if dbt State can reuse it based on your configuration rules. All reuses of the same active target table in a single day (based on UTC time) count as a single daily active target table (DATT).

### Monthly cost calculation

dbt State calculates cost per billing period using the unit price (USD $0.094) x sum of daily active target tables (DATT) for all account users and all days in that billing period. For example, if you have 100 DATT in a billing period, you'll be billed for 100 \* $0.094 = $9.40.

For current unit price and more information, refer to the [dbt Labs Service Consumption Table](https://www.getdbt.com/legal/service-consumption-table).

### DATT chart

You can view your DATT count in **Account settings** > **Billing & Usage** > **Usage-based features**. Under the **State** tab, the DATT chart shows DATTs split into **Billable** and **Free**. During a trial, all DATTs are counted as free. DATTs within your [monthly allowance](#free-monthly-datt-allowance) are also counted as free and do *not* appear on your invoice.

### Cancellation

Usage is tracked through your cancellation date. You're billed at month end for usage incurred before cancellation and not charged for usage after.

## Free monthly DATT allowance

Every account gets a monthly allowance of free daily active target tables (DATT). Usage within your allowance doesn’t appear on your invoice.

When your trial ends, you move to your plan's monthly allowance:

* Self-hosted dbt users move to the free plan, which includes 1,000 DATT each month indefinitely.
* Developer plan users also get 1,000 DATT each month indefinitely.
* Paid dbt platform plans get a temporary monthly allowance. Check out [paid plans with a temporary allowance](#paid-plans-with-a-temporary-allowance) for more details.

The allowance works the same way on every plan:

* It resets on the 1st of each month.
* Unused DATT don't carry over.
* Your first month gets the full allowance for your plan. Usage during your trial doesn't count against it.

#### Plans with a permanent allowance

| Plan                                   | Monthly allowance | When you reach the limit                                                                                                                                                           |
| -------------------------------------- | ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Self-hosted dbt (free plan), Developer | 1,000 DATT        | dbt State stops reusing nodes until the next reset. [Set up usage-based billing](./dbt-state-trial.md#setting-up-usage-based-billing) to resume. |

#### Paid plans with a temporary allowance

Paid dbt platform plans also get a monthly allowance for a limited time.

| Plan                                       | Monthly allowance | Available until   | When you reach the limit                                                                                                                                              |
| ------------------------------------------ | ----------------- | ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Starter                                    | 1,000 DATT        | March 31, 2027    | **Card on file:** dbt State keeps running and bills your card for usage past the limit.<br />**No card on file:** dbt State stops reusing nodes until the next reset. |
| Enterprise, Enterprise+, legacy Enterprise | 2,000 DATT        | March 31, 2027    | **Prepaid State contract:** dbt State keeps running and draws from your contract.<br />**No prepaid contract:** dbt State stops reusing nodes until the next reset.   |
| Legacy Teams                               | 1,000 DATT        | December 31, 2026 | dbt State stops reusing nodes until the next reset. To continue past the limit, migrate to Starter.                                                                   |

tip

On Starter, you can pause dbt State at the limit by turning off card billing for State on the **Usage-based features** page.

If you cancel paid access, dbt State keeps running on your plan's monthly allowance.

### Viewing your allowance

Go to **Account settings** > **Billing & Usage** > **Usage-based features** > **State** tab to see your current consumption, remaining allowance, and reset date.

dbt sends an email to your admin when you've used up 80% of your DATT allowance, and again if dbt State pauses at the limit.

## Setting up usage-based billing

dbt State pricing is usage-based; you're billed per target table that dbt State reuses each day, *not* per dbt platform seat.

You can set up billing during your trial or at any time after. Follow the steps for your account type:

#### Managed accounts

If your account has consumption spend on contract, go to the **State** tab of the **Usage-based features** page and click **Allow** to bill against your committed spend. Otherwise, [contact the dbt Labs sales team](https://www.getdbt.com/contact).

#### Self-hosted and Developer accounts

1. Go to **Billing & Usage** > **Usage-based features**.
2. Click **Set up billing** (if your trial is still active) or **Add billing info** (if your trial has ended).
3. In **Add a payment method**, enter your card details and billing address.
4. Click **Save card**.
5. Under the **State** tab, the card you added is displayed. Click **Confirm and activate**.

After setting up billing, you can optionally [set a spend alert](#setting-spend-alerts) to get notified when your monthly costs reach a threshold.

## Setting spend alerts

You can set a spend alert to get notified when your monthly dbt State costs reach a defined threshold.

1. In your dbt platform account, click your account name in the lower-left corner above your username and click **Account settings**.
2. Go to **Billing & Usage** > **Usage-based features**.
3. In the **Spend alert** section, click **Set a spend alert**.
4. Enable the toggle to receive email notifications when monthly spend reaches your threshold.
5. In the **Alert threshold** field, enter the amount in USD that triggers the alert.
6. Click **Save**.

## Related docs

* [About dbt State](./dbt-state-about.md)
* [Set up dbt State](./dbt-state-setup.md)
* [dbt State usage and pricing](../platform/billing.md#dbt-state-usage)
