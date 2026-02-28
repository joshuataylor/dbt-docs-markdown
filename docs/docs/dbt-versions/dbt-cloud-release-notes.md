# dbt release notes

dbt release notes for recent and historical changes. Release notes fall into one of the following categories:

* **New:** New products and features
* **Enhancement:** Performance improvements and feature enhancements
* **Fix:** Bug and security fixes
* **Behavior change:** A change to existing behavior that doesn't fit into the other categories, such as feature deprecations or changes to default settings

Release notes are grouped by month for both multi-tenant and virtual private cloud (VPC) environments.

## February 2026[​](#february-2026 "Direct link to February 2026")

* **Beta**: The `dbt-salesforce` adapter available in the dbt Fusion engine CLI is now in beta. For more information, see [Salesforce Data 360 setup](https://docs.getdbt.com/docs/fusion/connect-data-platform-fusion/salesforce-data-cloud-setup.md).

* **New**: You can connect to BigQuery using Workload Identity Federation (WIF) with external OAuth for [Semantic Layer queries](https://docs.getdbt.com/docs/use-dbt-semantic-layer/dbt-sl.md). For setup instructions, refer to [Set up BigQuery Workload Identity Federation](https://docs.getdbt.com/docs/cloud/manage-access/set-up-bigquery-oauth.md#set-up-bigquery-workload-identity-federation).

* **Enhancement**: For WIF-authenticated BigQuery connections, the Semantic Layer gateway refreshes access tokens on common expiration failures and recycles pooled connections to reduce authentication outages.

* **Enhancement:** The Analyst permission now has the project-level access to read repositories. See [Project access for project permissions](https://docs.getdbt.com/docs/cloud/manage-access/enterprise-permissions.md#project-access-for-project-permissions) for more information.

* **Enhancement:** After a user accepts an email [invite](https://docs.getdbt.com/docs/cloud/manage-access/invite-users.md) to access an [SSO-protected](https://docs.getdbt.com/docs/cloud/manage-access/sso-overview.md) dbt platform account, the UI now prompts them to log in with SSO to complete the process. This replaces the previous "Joined successfully" message, helping avoid confusion when users accept an invite but do not complete the SSO login flow.

* **New:** [Profiles](https://docs.getdbt.com/docs/cloud/about-profiles.md) let you define and manage connections, credentials, and attributes for deployment environments at the project level. dbt automatically creates profiles for existing projects and environments based on the current configurations, so you don't need to take any action. This is being rolled out in phases during the coming weeks.

* **New**: [Python UDFs](https://docs.getdbt.com/docs/build/udfs.md) are now supported and available in dbt Fusion engine when using Snowflake or BigQuery.

* **Enhancement:** Minor enhancements and UI updates to the Studio IDE, file explorer that replicate the VS Code IDE experience.

* **Enhancement:** Profile creation now displays specific validation error messages (such as "Profile keys cannot contain spaces or special characters") instead of generic error text, making it easier to identify and fix configuration issues.

* **Private beta**: [Cost Insights](https://docs.getdbt.com/docs/explore/cost-insights.md) shows estimated warehouse compute costs and run times for your dbt projects and models, directly in the dbt platform. It highlights cost reductions and efficiency gains from optimizations like [state-aware orchestration](https://docs.getdbt.com/docs/deploy/state-aware-about.md) across your project dashboard, model pages, and job details. See [Set up Cost Insights](https://docs.getdbt.com/docs/explore/set-up-cost-insights.md) and [Explore cost data](https://docs.getdbt.com/docs/explore/explore-cost-data.md) to learn more.

* **New**: The [dbt Semantic Layer](https://docs.getdbt.com/docs/use-dbt-semantic-layer/dbt-sl.md) now supports [Omni](https://docs.omni.co/integrations/dbt/semantic-layer) as a partner integration. For more info, see [Available integrations](https://docs.getdbt.com/docs/cloud-integrations/avail-sl-integrations.md).

* **Enhancement**: We clarified documentation for cumulative log size limits on run endpoints, originally introduced in [October 2025](https://docs.getdbt.com/docs/dbt-versions/2025-release-notes.md#october-2025). When logs exceed the cumulative size limit, dbt omits them and displays a banner. No functional changes were made in February 2026. For more information, see [Run visibility](https://docs.getdbt.com/docs/deploy/run-visibility.md#log-size-limits).

* **New**: The `immutable_where` configuration is now supported for Snowflake dynamic tables. For more information, see [Snowflake configurations](https://docs.getdbt.com/reference/resource-configs/snowflake-configs.md#immutable-where).

* **Fix**: The user invite details now show more information in invite status, giving admins visibility into users who accepted an invite to an SSO-protected account but haven't yet logged in via SSO. Previously, these invites were hidden, making it appear as if the user hadn't been invited. The Invites endpoints of the dbt platform Admin v2 API now include these additional statuses:

  <!-- -->

  * `4` (PENDINGEMAIL\_VERIFICATION)
  * `5` (EMAIL\_VERIFIED\_SSO).

* **Enhancement**: Improved performance on Runs endpoint for Admin V2 API and run details in dbt platform when connecting with GCP.

## January 2026[​](#january-2026 "Direct link to January 2026")

* **Enhancement:** The `defer-env-id` setting for choosing which deployment environment to defer to is [now available](https://docs.getdbt.com/docs/cloud/about-cloud-develop-defer.md#defer-environment) in the Studio IDE. Previously, this configuration only worked for the dbt CLI

* **Beta:** The [Analyst agent](https://docs.getdbt.com/docs/explore/navigate-dbt-insights.md#dbt-copilot) in dbt Insights is now in beta.

  * dbt Copilot's AI assistant in Insights now uses a dropdown menu to select between **Agent** and **Generate SQL**, replacing the previous tab interface.

* **Enhancement:** The [Studio IDE](https://docs.getdbt.com/docs/cloud/studio-ide/ide-user-interface.md#search-your-project) now includes search and replace functionality and a command palette, enabling you to quickly find and replace text across your project, navigate files, jump to symbols, and run IDE configuration commands. This feature is being rolled out in phases and will become available to all dbt platform accounts by mid-February.

* **Enhancement:** [State-aware orchestration](https://docs.getdbt.com/docs/deploy/state-aware-about.md) improvements:

  * When a model fails a data test, state-aware orchestration rebuilds it on subsequent runs instead of reusing it from prior state to ensure dbt reevaluates data quality issues.
  * State-aware orchestration now detects and rebuilds models whose tables are deleted from the warehouse, even when there are no code or data changes. Previously, tables deleted externally were not detected, and therefore not rebuilt, unless code or data had changed. For more information, see [Handling deleted tables](https://docs.getdbt.com/docs/deploy/state-aware-about.md#handling-deleted-tables).

  State-aware orchestration is in private preview. See the [prerequisites for using the feature](https://docs.getdbt.com/docs/deploy/state-aware-setup.md#prerequisites).

* **Enhancement:** [dbt Copilot](https://docs.getdbt.com/docs/cloud/dbt-copilot.md) correctly detects column names across various `schema.yml` files, adds only missing descriptions, and preserves existing ones.

* **Enhancement**: The Fusion CLI now automatically reads environment variables from a `.env` file in your current working directory (the folder you `cd` into and run dbt commands from in your terminal), if one exists. This provides a simple way to manage credentials and configuration without hardcoding them in your `profiles.yml`. The [dbt VS Code extension](https://docs.getdbt.com/docs/about-dbt-extension.md) also supports `.env` files as well as LSP-powered features. For more information, refer to [Install Fusion CLI](https://docs.getdbt.com/docs/fusion/install-fusion-cli.md#environment-variables).

* **New**: The new Semantic Layer YAML specification creates an open standard for defining metrics and dimensions that works across multiple platforms. The new spec is now live in the dbt Fusion engine.

  Key changes:

  * Semantic models are now embedded within model YAML entries. This removes the need to manage YAML entries across multiple files.
  * Measures are now simple metrics.
  * Frequently used options are now top-level keys, reducing YAML nesting depth.

  For an overview of the changes and steps how to migrate to the latest YAML spec, see [Migrate to the latest YAML spec](https://docs.getdbt.com/docs/build/latest-metrics-spec.md).

* **Fix:** Debug logs in the **Run summary** tab are now properly truncated to improve performance and user interface responsiveness. Previously, debug logs were not truncated correctly, causing slower page loads. You can access the full debug logs by clicking **Download > Download all debug logs**. For more information, see [Run visibility](https://docs.getdbt.com/docs/deploy/run-visibility.md#run-summary-tab).

* **New:** The [Semantic Layer querying](https://docs.getdbt.com/docs/explore/navigate-dbt-insights.md#semantic-layer-querying) within dbt Insights is now generally available (GA), enabling you to build SQL queries against the Semantic Layer without writing SQL code.

* **Enhancement**: Eligible dbt platform accounts in the Fusion private preview can now use [Exposures](https://docs.getdbt.com/docs/cloud-integrations/downstream-exposures.md).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
