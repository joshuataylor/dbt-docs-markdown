# Set up Cost Insights [Private beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

This guide walks you through setting up Cost Insights to track warehouse compute costs and cost reductions from state-aware orchestration across your dbt projects and models.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

Before setting up Cost Insights, ensure you have:

* A dbt account with dbt Fusion engine enabled. Contact your account manager to enable Fusion for your account.
* An administrator role.
* A supported data warehouse: Snowflake, BigQuery, or Databricks.

To set up Cost Insights, refer to the following steps:

1. [Assign required permissions.](#assign-required-permissions)
2. [Configure platform metadata credentials.](#configure-platform-metadata-credentials)
3. [(Optional) Configure Cost Insights settings.](#configure-cost-insights-settings-optional)
4. [(Optional) Enable state-aware orchestration in your job settings.](#enable-state-aware-orchestration-optional)

After completing these setup steps, you can view cost and optimization data across multiple areas of the dbt platform. Refer to [Explore cost data](https://docs.getdbt.com/docs/explore/explore-cost-data.md) to learn more about the Cost Insights section and how to use it.

## Assign required permissions[​](#assign-required-permissions "Direct link to Assign required permissions")

Users with the following [permission sets](https://docs.getdbt.com/docs/cloud/manage-access/enterprise-permissions.md) can view cost data by default:

* Account Admin
* Account Viewer
* Cost Insights Admin
* Cost Insights Viewer
* Database Admin
* Git Admin
* Job Admin
* Project Creator
* Team Admin

For more information on how to assign permissions to users, refer to [About user access](https://docs.getdbt.com/docs/cloud/manage-access/about-user-access.md).

## Configure platform metadata credentials[​](#configure-platform-metadata-credentials "Direct link to Configure platform metadata credentials")

1. Click your account name at the bottom of the left-side menu and click **Account settings**.

2. Under **Settings**, go to **Connections**.

3. Select an existing connection or create a new connection for the project where you want to enable Cost Insights.

4. Enable platform metadata credentials for your connection.

   <!-- -->

   1. Go to the **Platform metadata credentials** section and click **Add credentials**.

   2. Add credentials with permissions to the warehouse tables. Expand each connection to see the permissions required.

       Snowflake

      * `read` permissions to the [`ORGANIZATION_USAGE`](https://docs.snowflake.com/en/sql-reference/organization-usage) and [`ACCOUNT_USAGE`](https://docs.snowflake.com/en/sql-reference/account-usage) schemas

      * A Snowflake database role assigned the following access:

        <!-- -->

        * `ACCOUNT_USAGE.QUERY_HISTORY`
        * `ACCOUNT_USAGE.QUERY_ATTRIBUTION_HISTORY`
        * `ACCOUNT_USAGE.ACCESS_HISTORY`
        * `ACCOUNT_USAGE.WAREHOUSE_METERING_HISTORY`
        * `ORGANIZATION_USAGE.USAGE_IN_CURRENCY_DAILY` (Optional)

       BigQuery

      * `bigquery.datasets.get`
      * `bigquery.jobs.create`
      * `bigquery.jobs.listAll`

       Databricks

      * Access to a [Unity Catalog workspace](https://docs.databricks.com/aws/en/admin/system-tables/#requirements)

      * `USE` permissions on the catalog and schema

      * `SELECT` permissions on the following system tables:

        <!-- -->

        * [`system.billing`](https://docs.databricks.com/aws/en/admin/system-tables/billing)
        * [`system.pricing`](https://docs.databricks.com/aws/en/admin/system-tables/pricing)
        * [`system.query_history`](https://docs.databricks.com/aws/en/admin/system-tables/query-history)

      For more information, see the Databricks documentation on [granting access to system tables](https://docs.databricks.com/aws/en/admin/system-tables/#grant-access-to-system-tables).

      If you have multiple connections that reference the same account identifier, you will only be prompted to add platform metadata credentials to one of them. Other connections using the same account identifier will display a message indicating that credentials are already configured.

5. Verify that **Cost Insights** is enabled under **Features**. This feature is enabled by default when you configure platform metadata credentials.

6. Click **Save**.

## Configure Cost Insights settings (optional)[​](#configure-cost-insights-settings-optional "Direct link to Configure Cost Insights settings (optional)")

By default, dbt uses standard warehouse pricing. If you have custom pricing contracts, you can override these values *except* for Databricks connections. The default values vary by warehouse:

| Warehouse                                                                    | Default values                                                                     |
| ---------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- |
| [Snowflake](https://www.snowflake.com/en/pricing-options/)                   | `price_per_credit` = $3                                                            |
| [BigQuery](https://cloud.google.com/bigquery/pricing)                        | `price_per_slot_hour` = $0.04, `price_per_tib` = $6.25                             |
| [Databricks](https://docs.databricks.com/aws/en/admin/system-tables/pricing) | dbt queries the `list_prices` system table directly, so there is no default value. |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

<br />

To change the default value:

1. Click your account name at the bottom of the left-side menu and click **Account settings**.
2. Under **Settings**, go to **Connections**.
3. Select the connection where you want to configure Cost Insights settings.
4. Go to the **Cost Insights settings** section.
5. Enter your custom value in the **Price per credit** field.
6. Click **Save**.

These custom values will apply to all future cost calculations for this connection. If you clear these values, they will reset to the default warehouse pricing.

## Enable state-aware orchestration (optional)[​](#enable-state-aware-orchestration-optional "Direct link to Enable state-aware orchestration (optional)")

Cost Insights displays cost data for your dbt models and jobs without state-aware orchestration. However, to understand the impact of optimizations and see cost reductions from model and test reuse, you must enable state-aware orchestration in your jobs. For steps on how to enable this feature, see [Setting up state-aware orchestration](https://docs.getdbt.com/docs/deploy/state-aware-setup.md).

<!-- -->

note

For accounts already using state-aware orchestration before Cost Insights is enabled, at least one full model build must occur within the last 10 days to establish a baseline for cost reduction calculations. If you don't see cost reduction data, try running a full build to establish the baseline.

## Disable Cost Insights[​](#disable-cost-insights "Direct link to Disable Cost Insights")

To disable Cost Insights, you must have an administrator role.

1. Click your account name at the bottom of the left-side menu and click **Account settings**.
2. Under **Settings**, go to **Connections**.
3. Select the connection where you want to disable Cost Insights.
4. Go to **Platform metadata credentials** and click **Edit**.
5. Go to the **Features** section and clear the **Cost Insights** option.
6. Click **Save**.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
