# dbt release notes

dbt release notes for recent and historical changes. Release notes fall into one of the following categories:

* **New:** New products and features
* **Enhancement:** Performance improvements and feature enhancements
* **Fix:** Bug and security fixes
* **Behavior change:** A change to existing behavior that doesn't fit into the other categories, such as feature deprecations or changes to default settings

Release notes are grouped by month for both multi-tenant and virtual private cloud (VPC) environments.

For dbt Fusion engine updates, refer to the [dbt-fusion changelog](https://github.com/dbt-labs/dbt-fusion/blob/main/CHANGELOG.md).

## June 2026[​](#june-2026 "Direct link to June 2026")

* **Enhancement**: You can now configure [dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md) for the Studio IDE directly in the dbt platform UI — either as a team-wide default on your development environment, or as a personal override. For more information, refer to [Enabling dbt State in Studio](https://docs.getdbt.com/docs/deploy/dbt-state-setup.md#enabling-dbt-state-in-studio).
* **New:** [Model query history](https://docs.getdbt.com/docs/explore/model-query-history.md) for Redshift and Databricks is now generally available (GA).
* **Behavior change:** On June 29, 2026, several behavior change flags on the dbt platform **Latest** release track will reach maturity (enabled by default). Refer to [Flags reaching maturity](https://docs.getdbt.com/reference/global-configs/behavior-flag-maturity.md#flags-reaching-maturity) to see which flags may affect your project and how to opt out before then.
* **Beta:** The dbt Fusion engine now supports the Salesforce Data 360 connection in the dbt platform. For more information, refer to [Connect Salesforce Data 360](https://docs.getdbt.com/docs/platform/connect-data-platform/connect-salesforce.md).
* **Private beta**: The [Analyst read](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions.md#analyst-read) permission set is available for Enterprise plans.
  <!-- -->
  * *New*: Analyst read is a project-level permission set that provides read-only access to analyze dbt models and project resources. The OAuth integration that lets read-only users connect to analysis features (such as the [dbt MCP server](https://docs.getdbt.com/docs/dbt-ai/about-mcp.md)) is available to use, while the Analyst read permission set and read-only permission changes are in private beta. To enable them, contact your account manager.
* **Beta**: Workspace-level Private Link for Microsoft Fabric is now available in beta. Configure a private connection between the dbt platform and your Fabric workspace so SQL traffic stays on Azure's private network. For more information, refer to [Configuring Private Link for Microsoft Fabric](https://docs.getdbt.com/docs/platform/secure/private-connectivity/azure/azure-fabric.md).
* **Beta**: [Cost Insights](https://docs.getdbt.com/docs/explore/cost-insights.md) now supports Amazon Redshift Serverless and provisioned clusters. Configure your platform metadata credentials with the `sys:monitor` role or `SYSLOG ACCESS UNRESTRICTED` permission to allow dbt to read cross-user query history, then set your pricing in Cost Insights settings. For more information, refer to [Set up Cost Insights](https://docs.getdbt.com/docs/explore/set-up-cost-insights.md).

### Snowflake Summit 2026 announcements[​](#snowflake-summit-2026-announcements "Direct link to Snowflake Summit 2026 announcements")

The following features are new or enhanced as part of dbt Labs announcements at [Snowflake Summit 2026](https://www.getdbt.com/events/snowflake-summit-2026) in San Francisco from June 1–4, 2026:

* **Alpha**: [dbt Core v2.0](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md) is now available in alpha!

  * **New**: dbt Core v2.0 is the open-source Apache 2.0 foundation that the dbt Fusion engine builds on, delivering a faster, Rust-based runtime. It ships as two distributions: `dbt-core` (OSS, Apache 2.0) and `dbt` (Fusion distribution, proprietary).

* **Beta**: [`dbt lint`](https://docs.getdbt.com/reference/commands/lint.md) is now available in beta!

  * **New**: `dbt lint` is a high-performance SQL linter built into the dbt platform, available on projects running the dbt Fusion engine. It is SQLFluff-compatible; it reads your existing `.sqlfluff` config, uses the same rule codes, and respects `-- noqa` suppression comments. In benchmarks, it runs roughly 50× faster than single-threaded SQLFluff..

* **Preview**: [dbt Docs v2](https://docs.getdbt.com/docs/build/view-documentation.md#dbt-docs-v2) is now available in preview!

  * **New**: dbt Docs v2 is a next-generation open-source catalog experience available with the dbt Fusion engine and dbt Core v2. It uses a compact binary index instead of loading the full `manifest.json` in the browser, making it significantly faster for large projects.
  * **New**: dbt Docs v2 includes a redesigned UI, Semantic Layer metadata, column-level lineage (Fusion only), and a REST API at `/api/v1/` so AI agents and MCP servers can query your dbt project metadata without a browser.
  * **New**: Generate and serve [dbt Docs v2](https://docs.getdbt.com/docs/build/view-documentation.md#dbt-docs-v2) with the dbt Fusion engine or dbt Core v2 by running a dbt command with `--use-index`, then `dbt docs serve`. Add [`--write-catalog`](https://docs.getdbt.com/reference/commands/cmd-docs.md#--write-catalog-flag) for richer column type metadata.

* **Preview**: [dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md) is now available in preview!

  * **New**: dbt State skips or clones nodes when the logic and data haven't changed, rather than rebuilding everything on every run. Available in dbt Core v1.12+, dbt v2.0, the dbt platform, and the dbt Fusion engine. To get started, refer to [Set up dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-setup.md).
  * **New**: [dbt State pricing](https://docs.getdbt.com/docs/platform/billing.md#dbt-state-usage) is usage-based at $0.094 per daily unique reuse. New organizations receive a 30-day free trial with no usage limit.
  * **Behavior change**: State-aware orchestration is no longer being enabled for new customers. Refer to [Migrate to dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-migration.md) for more information.

* **New**: dbt Wizard is available in dbt platform as a public preview. Introducing dbt Wizard CLI as a public beta. Purpose-built for agentic governed data development in dbt, dbt Wizard understands your project through a [native metadata engine](https://docs.getdbt.com/docs/dbt-ai/wizard-how-it-works.md#native-metadata-engine), unlike general-purpose coding agents.

  * **New**: [Support for Anthropic as a BYOK provider for dbt AI](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md#configure-your-ai-provider).
  * **New**: [`dbt login`](https://docs.getdbt.com/reference/commands/login.md) is a new CLI command available in dbt Core v2.0 and later. It opens browser-based authentication and shares your login state across the CLI, dbt VS Code extension, dbt State, and dbt Wizard CLI with no separate sign-in flows needed.

* **New:** OAuth client registrations now accept custom-scheme redirect URIs (for example, `cursor://` or `vscode://`), so you can build native app OAuth integrations with Cursor and VS Code.

* **New:** Public REST API endpoints at `/api/ide/v3/{environment_id}/files/` support Studio IDE workspace file operations, including stat, read, write, list, delete, mkdir, and rename. Pass file paths as query parameters.

* **New:** The `GET /api/ide/v3/{environment_id}/status` endpoint returns the `dbt_version` and `is_fusion` status for a given environment.

* **New:** The dbt CLI Python client's `create_invocation()` method now supports a `workspace` parameter, so you can run invocations against persisted workspace files on workers.

## May 2026[​](#may-2026 "Direct link to May 2026")

* **Fix:** The connection test failure message now prompts you to verify your connection details and confirm that your credentials have access to the data warehouse, rather than showing a generic failure message.
* **Enhancement:** Users granted `user_credential_write` can access **Your profile** > **Credentials** without `develop_access` (including read-only users). Environment variable overrides and dbt version overrides still require `develop_access`. Refer to [Enterprise permissions](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions.md) for more information.
* **New:** The [Job creator permission set](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions.md#job-creator) is now available for Enterprise accounts. Assign it to users who need to create, edit, and run jobs within assigned projects and environments without access to edit environments or environment variables.
* **Enhancement:** The admin API toolset (job management and run operations) is now always available in the dbt
  <!-- -->
  [](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md)and no longer requires a feature flag. You no longer need to contact your account manager to enable these tools.
* **Fix:** When a job cannot clone its repository because no remote URL is configured, the error message now explains the most likely causes (an invalid Git remote URL, a Git provider outage, or a deprecated HTTPS connection) and directs you to verify the URL, confirm your provider is operational, and ensure the repository uses SSH with deploy keys before retrying.
* **New:** The **Notification Manager** [permission set](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions.md) is now available for Enterprise accounts. Assign it to users who need to manage Slack, Microsoft Teams, and email job notifications across all projects without requiring full Account Admin access.
* **Beta**: [Cost Insights](https://docs.getdbt.com/docs/explore/cost-insights.md), available in public beta, shows estimated warehouse compute costs and run times for dbt projects and models in dbt platform, highlighting efficiency gains from [state-aware orchestration](https://docs.getdbt.com/docs/deploy/state-aware-about.md). Refer to [Set up Cost Insights](https://docs.getdbt.com/docs/explore/set-up-cost-insights.md) and [Explore cost data](https://docs.getdbt.com/docs/explore/explore-cost-data.md) to learn more.
* **New:** Fusion release tracks are now being rolled out across across accounts in phases. Refer to [Fusion release tracks](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md?version=2.0#fusion-release-tracks) for more information.
* **Enhancement:** Commands run by
  <!-- -->
  and the [](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md)now appear in the Studio IDE **Commands** tab with a
  <!-- -->
  icon and **Run by Copilot** tooltip, so you can tell agent-run commands apart from manually run ones.
* **Fix:** [`state:modified`](https://docs.getdbt.com/reference/node-selection/methods.md#state) now detects changes to [UDF](https://docs.getdbt.com/docs/build/udfs.md) properties (such as `arguments` and `returns`) defined in `.yml` files. Previously, only changes to the SQL or Python function body were detected.
* **New:** [Native private packages](https://docs.getdbt.com/docs/build/packages.md#native-private-packages) are now generally available (GA).
* **Preview**: The [Developer agent](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md) is now in preview. Use natural language prompts to build or refactor models, and generate SQL, tests, documentation, and semantic models from scratch. For more information, refer to the [Developer agent](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md).
* **Behavior change:** When you set up single sign-on (SSO) in the dbt platform, the SSO slug is now system-generated and read-only. Existing SSO configurations remain valid, but you can’t change the slug. If you delete and recreate your SSO configuration, the new configuration uses a new, system-generated slug. Refer to [Single sign-on overview](https://docs.getdbt.com/docs/platform/manage-access/sso-overview.md) for more information.
* **Enhancement:** The [dbt VS Code extension](https://docs.getdbt.com/docs/install-dbt-extension.md?version=2.0) now supports account creation. If you sign in with an existing dbt user that doesn't have an associated dbt platform account, the registration flow prompts you to create one instead of requiring a separate workflow.
* **Enhancement:** Delete individual [dbt Wizard chat conversations](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md#availability-and-considerations) from the conversation list (three dots → **Delete**). Deleting the open conversation clears the panel.
* **New:** The Fusion + Snowflake connection experience is now generally available on the dbt platform. See our [Fusion upgrade guides](https://docs.getdbt.com/guides/prepare-fusion-upgrade.md?step=1) for information on enabling the upgrade workflows for your environments today!
* **Enhancement:** In the Discovery API [Tests object schema](https://docs.getdbt.com/docs/dbt-apis/discovery-schema-environment-applied-tests.md), you can now filter `environment.applied.tests` by multiple test result statuses in a single query using the new `lastKnownResults: [TestStatus]` filter field on `TestAppliedFilter`. The single-value `lastKnownResult` filter field is still supported but deprecated. Update your queries to use `lastKnownResults` going forward.
* **Enhanced** Fusion eligibility job prompts now use a **Debug on Fusion** dropdown instead of a standalone **Run once on Fusion** button. For more information, refer to [Update your jobs](https://docs.getdbt.com/guides/prepare-fusion-upgrade.md?step=7).
* **Enhancement:** The [](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md)input bar now supports arrow key history navigation. Press the up arrow at the start of the input to cycle through previous inputs, and the down arrow at the end to return to more recent ones. dbt stores up to 5 previous inputs per session.
* **Enhancement:** Tool approval and file edit dialogs in the [](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md)now support number key shortcuts (1, 2, 3) to select options. The first option is auto-focused when a dialog appears, so you can act immediately without clicking.

## April 2026[​](#april-2026 "Direct link to April 2026")

* **Enhancement:** When a dbt command run by the [](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md)times out, the agent now automatically attempts to cancel the stuck invocation on the server and returns a retry-friendly message, letting you decide whether to retry. Previously, timeouts resulted in an unhandled error. This applies to both model invocations and autofix runs.
* **Enhancement:** In dbt platform run logs, `dbt ls` and `dbt list` now display node results as **No-op** instead of **Unknown** when using dbt Fusion engine. Refer to [dbt ls (list)](https://docs.getdbt.com/reference/commands/list.md) for more information.
* **New:** A universal login URL is available at <https://login.dbt.com>, making it easier for you to view accounts you have access to across instances (regions and tenancies). This is currently available for multi-tenant accounts with an account-specific domain, and support for single-tenant accounts is coming soon. For more information, refer to [Log in to dbt platform](https://docs.getdbt.com/docs/platform/about-platform/login.md).
* **Fix:** Refreshing the same browser tab now restores your active dbt Wizard conversation instead of showing the empty state. Opening a new tab, or returning after closing the tab, still starts in the empty state. The dbt Wizard is currently in beta.
* **Enhancement:** The dbt VS Code extension's **Get started** panel has been redesigned and surfaces the exact next setup step you need to install the extension and Fusion. The new panel also supports a new **agentic migration** option that helps you upgrade your project to Fusion automatically in Copilot or Cursor. For more info, see [Getting started](https://docs.getdbt.com/docs/install-dbt-extension.md#getting-started).
* **Beta**: [Model query history](https://docs.getdbt.com/docs/explore/model-query-history.md) now also supports Databricks and Redshift. Refer to [Credential permissions](https://docs.getdbt.com/docs/explore/model-query-history.md#credential-permissions) for more information.
* **Enhancement:** [Slack notifications (account-level)](https://docs.getdbt.com/docs/deploy/job-notifications.md#slack-notifications-account) and [Microsoft Teams notifications](https://docs.getdbt.com/docs/deploy/job-notifications.md#microsoft-teams-notifications) are now generally available, enabling you to send job notifications directly to Slack channels configured at the account level, and to Teams channels.
* **Enhancement:** When using the [dbt autofix](https://github.com/dbt-labs/dbt-autofix) tool in the Studio IDE, you can now compile your project directly from the results panel after a successful `dbt parse`. Click **Compile** next to the **Successfully resolved** result to kick off a compile. For more information, refer to [Fix deprecation warnings](https://docs.getdbt.com/docs/platform/studio-ide/autofix-deprecations.md).
* **Beta**: DuckDB is now supported in the dbt Fusion engine CLI, which lets you run local dbt projects without a warehouse account. For more information, refer to [Connect DuckDB](https://docs.getdbt.com/docs/local/connect-data-platform/duckdb-setup.md).
* **New**: You can now configure Snowflake PrivateLink endpoints directly in dbt platform without contacting dbt Support, available in private beta. Go to **Account settings → Integrations → Private endpoints** to request and manage Snowflake PrivateLink endpoints on AWS. This feature is available for Snowflake on AWS only. For more information, refer to [AWS PrivateLink for Snowflake](https://docs.getdbt.com/docs/platform/secure/private-connectivity/aws/aws-snowflake.md?version=1.12).
* **Enhancement:** You can now use arrays as values for keys in the dbt platform extended attributes YAML editor. For example, `db_groups: [db_editor, db_viewer]` is now valid. Previously, array values were only supported using the API. For more information, refer to [Extended attributes](https://docs.getdbt.com/docs/dbt-platform-environments.md#extended-attributes).
* **Beta**: The Redshift adapter now supports a `datasharing` profile credential on the dbt platform **Latest** release track. When set to `true`, dbt uses Redshift's native `SHOW` commands (for example, `SHOW TABLES`, `SHOW COLUMNS`, `SHOW SCHEMAS`) for metadata queries instead of PostgreSQL catalog tables, enabling cross-database and cross-cluster access with [Redshift Datasharing](https://docs.aws.amazon.com/redshift/latest/dg/datashare-overview.html). For more information, refer to [Redshift setup](https://docs.getdbt.com/docs/local/connect-data-platform/redshift-setup.md#datasharing).
* **Enhancement:** When a connection does not have platform metadata credentials configured yet, the credentials form now renders in edit mode immediately — you no longer need to click **Add credentials** first. If you cancel, the **Add credentials** button appears so you can return to the form. Existing connections with configured platform metadata credentials are unaffected. Refer to [Configure the warehouse connection](https://docs.getdbt.com/docs/explore/external-metadata-ingestion.md#configure-the-warehouse-connection) for more information.
* **New**: The [dbt Remote dbt MCP server](https://docs.getdbt.com/docs/dbt-ai/about-mcp.md?version=2.0) now supports Admin API calls! This allows users to troubleshoot job-related errors in agents like Claude and Cursor.
* **New**: The [Developer agent](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md) is now in beta. Use the Developer agent to write or refactor dbt models from natural language, generate documentation, tests, semantic models, and SQL code from scratch, giving you the flexibility to modify or fix generated code. For more information, refer to the [Developer agent](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md).
* **Enhancement:** The Studio IDE now validates dbt YAML using Fusion aligned JSON Schema from [dbt-jsonschema](https://github.com/dbt-labs/dbt-jsonschema) across [dbt platform release tracks](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md), including for development environments on dbt Core. This improves autocomplete and structural feedback in the editor. Diagnostics can occasionally disagree with what your environment accepts; use dbt runs and previews as the source of truth. For context, review [Migrate to the latest YAML spec](https://docs.getdbt.com/docs/build/latest-metrics-spec.md) and [dbt YAML validation in Studio](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md#dbt-yaml-validation). This will be a phased rollout starting the week of April 6th.
* **Enhancement:** The Studio IDE status bar now offers more control, more detailed information, and quicker access to settings for deferral, dbt version, and project status. For more information, refer to the [Studio IDE docs](https://docs.getdbt.com/docs/platform/studio-ide/ide-user-interface.md#the-command-and-status-bar). These updates roll out in phases to existing accounts starting April 6.
* **Enhancement:** In Snowflake **Private endpoints**, output validation errors now display inline beneath the text area (instead of as a page-level banner). The **Submit request** button is also disabled when the output is invalid (for example, empty, malformed JSON, or missing required fields).
* **Enhancement:** The Studio IDE now supports deep links to a specific console tab using the `?consoleTab=` query parameter. For example, append `?consoleTab=problems` to open Studio with the **Problems** tab pre-selected. The `problems` tab applies only when it is available for the current session.

## March 2026[​](#march-2026 "Direct link to March 2026")

* **Enhancement:** The environment [Connection profiles](https://docs.getdbt.com/docs/platform/about-profiles.md#environment-profiles-table) page has been updated. The profile name is now a clickable button that opens the view/edit drawer, the Connection column links to the connection details page in a new tab, and in edit mode a **swap icon** button lets you change the assigned profile. The previous ellipsis menu has been removed. For details, refer to [About profiles](https://docs.getdbt.com/docs/platform/about-profiles.md).
* **Beta:** Apache Spark is now supported in the dbt Fusion engine CLI, enabling faster compilation and execution for Spark-based dbt projects. Fusion currently supports only Apache Spark 3.0. For more information, refer to [Connect Apache Spark to Fusion](https://docs.getdbt.com/docs/local/connect-data-platform/spark-setup.md).
* **Enhancement:** [Cost Insights](https://docs.getdbt.com/docs/explore/cost-insights.md) charts now include an **Assets** filter (**Models** / **Tests** / **All**) on the **Cost**, **Usage**, **Query run time**, and **Builds** tabs. Use the dropdown on each chart to filter the data you want to view; your selection is stored per tab. The former **Model builds** tab is now labeled **Builds**. For more information, refer to [Explore cost data](https://docs.getdbt.com/docs/explore/explore-cost-data.md).
* **Enhancement:** [Deferral](https://docs.getdbt.com/reference/node-selection/defer.md) now supports [user-defined functions (UDFs)](https://docs.getdbt.com/docs/build/udfs.md). When you run a dbt command with `--defer` and `--state`, dbt resolves `function()` calls from the state manifest. This lets you run models that depend on UDFs without first building those UDFs in your current target.
* **Fix**: Status messages that exceed the 1024 character limit are now automatically truncated to prevent validation errors and run timeouts. Previously, long status messages could cause runs to fail with unhandled exceptions or result in lost status information. The system now logs when truncation occurs to help identify and optimize verbose status messages.
* **Fix:** Resolved an issue where [retrying failed runs](https://docs.getdbt.com/docs/deploy/retry-jobs.md) that were triggered from Git tags would use the wrong commit. Previously, when runs were triggered from Git tags instead of branches, the system would enter a detached HEAD state, causing retries to use the latest commit on HEAD rather than the original tagged commit. The fix now correctly preserves and uses the original Git tag reference when retrying runs, ensuring consistency between the initial run and any retries.
* **New**: The [dbt MCP server](https://docs.getdbt.com/docs/dbt-ai/about-mcp.md?version=2.0#product-docs) now includes product docs tools (`search_product_docs` and `get_product_doc_pages`) that let your AI assistant search and fetch pages from docs.getdbt.com in real time. Get responses grounded in the latest official dbt documentation rather than relying on training data or web searches, so you can stay in your development flow and trust the answers. This allows you to stay in your development flow and trust. These tools are enabled by default with no additional configuration. Restart your MCP server if you don't see the product docs tools in your MCP config. For more information, refer to [the dbt MCP repo](https://github.com/dbt-labs/dbt-mcp?tab=readme-ov-file#product-docs).
* **Enhancement**: The Model Timing tab displays an informative banner for dbt Fusion engine runs instead of the timing chart. The banner explains "Model timing is not yet available for Fusion runs" and provides context about threading differences. Non-Fusion runs continue to show the timing chart normally.
* **Behavior change**: [Snowflake plans to increase](https://docs.snowflake.com/en/release-notes/bcr-bundles/un-bundled/bcr-2118) the default column size for string and binary data types in September 2026. `dbt-snowflake` versions below v1.10.6 may fail to build certain incremental models when this change is deployed. [Assess impact and take any required actions](https://docs.getdbt.com/reference/resource-configs/snowflake-configs.md#assess-impact-and-required-actions).
* **New**: The new Semantic Layer YAML specification is now available on the dbt platform **Latest** release track. For an overview of the changes and steps how to migrate to the latest YAML spec, refer to [Migrate to the latest YAML spec](https://docs.getdbt.com/docs/build/latest-metrics-spec.md).
* **Behavior change:** New projects in trial, starter, or Enterprise accounts now default to **Fusion Stable** for all new environments with a supported adapter (Redshift, Snowflake, BigQuery, and Databricks). You can revert to another version by changing the dbt version in your [environment settings](https://docs.getdbt.com/docs/dbt-platform-environments.md#change-environment-settings).

## February 2026[​](#february-2026 "Direct link to February 2026")

* **New**: Advanced CI (dbt compare in orchestration) is now supported in the dbt Fusion engine. For more information, review [Advanced CI](https://docs.getdbt.com/docs/deploy/advanced-ci.md).

* **Beta**: The `dbt-salesforce` adapter available in the dbt Fusion engine CLI is now in beta. For more information, refer to [Salesforce Data 360 setup](https://docs.getdbt.com/docs/fusion/connect-data-platform-fusion/salesforce-data-cloud-setup.md).

* **Enhancement:** The Analyst permission now has the project-level access to read repositories. Review [Project access for project permissions](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions.md#project-access-for-project-permissions) for more information.

* **Enhancement:** After a user accepts an email [invite](https://docs.getdbt.com/docs/platform/manage-access/invite-users.md) to access an [SSO-protected](https://docs.getdbt.com/docs/platform/manage-access/sso-overview.md) dbt platform account, the UI now prompts them to log in with SSO to complete the process. This replaces the previous "Joined successfully" message, helping avoid confusion when users accept an invite but do not complete the SSO login flow.

* **New:** [Profiles](https://docs.getdbt.com/docs/platform/about-profiles.md) let you define and manage connections, credentials, and attributes for deployment environments at the project level. dbt automatically creates profiles for existing projects and environments based on the current configurations, so you don't need to take any action. This is being rolled out in phases during the coming weeks.

* **New**: [Python UDFs](https://docs.getdbt.com/docs/build/udfs.md) are now supported and available in dbt Fusion engine when using Snowflake or BigQuery.

* **Enhancement:** Minor enhancements and UI updates to the Studio IDE, file explorer that replicate the VS Code IDE experience.

* **Enhancement:** Profile creation now displays specific validation error messages (such as "Profile keys cannot contain spaces or special characters") instead of generic error text, making it easier to identify and fix configuration issues.

* **Private beta**: [Cost Insights](https://docs.getdbt.com/docs/explore/cost-insights.md) shows estimated warehouse compute costs and run times for your dbt projects and models, directly in the dbt platform. It highlights cost reductions and efficiency gains from optimizations like [state-aware orchestration](https://docs.getdbt.com/docs/deploy/state-aware-about.md) across your project dashboard, model pages, and job details. Refer to [Set up Cost Insights](https://docs.getdbt.com/docs/explore/set-up-cost-insights.md) and [Explore cost data](https://docs.getdbt.com/docs/explore/explore-cost-data.md) to learn more.

* **New**: The [dbt Semantic Layer](https://docs.getdbt.com/docs/use-dbt-semantic-layer/dbt-sl.md) now supports [Omni](https://docs.omni.co/integrations/dbt/semantic-layer) as a partner integration. For more information, refer to [Available integrations](https://docs.getdbt.com/docs/platform-integrations/avail-sl-integrations.md).

* **Enhancement**: We clarified documentation for cumulative log size limits on run endpoints, originally introduced in [October 2025](https://docs.getdbt.com/docs/dbt-versions/2025-release-notes.md#october-2025). When logs exceed the cumulative size limit, dbt omits them and displays a banner. No functional changes were made in February 2026. For more information, review [Run visibility](https://docs.getdbt.com/docs/deploy/run-visibility.md#log-size-limits).

* **New**: The `immutable_where` configuration is now supported for Snowflake dynamic tables. For more information, refer to [Snowflake configurations](https://docs.getdbt.com/reference/resource-configs/snowflake-configs.md#immutable-where).

* **Fix**: The user invite details now show more information in invite status, giving admins visibility into users who accepted an invite to an SSO-protected account but haven't yet logged in via SSO. Previously, these invites were hidden, making it appear as if the user hadn't been invited. The Invites endpoints of the dbt platform Admin v2 API now include these additional statuses:

  <!-- -->

  * `4` (PENDINGEMAIL\_VERIFICATION)
  * `5` (EMAIL\_VERIFIED\_SSO).

* **Enhancement**: Improved performance on Runs endpoint for Admin V2 API and run details in dbt platform when connecting with GCP.

## January 2026[​](#january-2026 "Direct link to January 2026")

* **Enhancement:** The `defer-env-id` setting for choosing which deployment environment to defer to is [now available](https://docs.getdbt.com/docs/platform/about-defer.md#configure-deferral-environment-id) in the Studio IDE. Previously, this configuration only worked for the dbt CLI

* **Beta:** The [Analyst agent](https://docs.getdbt.com/docs/explore/navigate-dbt-insights.md#dbt-copilot) in dbt Insights is now in beta.

  * dbt dbt Wizard's AI assistant in Insights now uses a dropdown menu to select between **Agent** and **Generate SQL**, replacing the previous tab interface.

* **Enhancement:** The [Studio IDE](https://docs.getdbt.com/docs/platform/studio-ide/ide-user-interface.md#search-your-project) now includes search and replace functionality and a command palette, enabling you to quickly find and replace text across your project, navigate files, jump to symbols, and run IDE configuration commands. This feature is being rolled out in phases and will become available to all dbt platform accounts by mid-February.

* **Enhancement:** [State-aware orchestration](https://docs.getdbt.com/docs/deploy/state-aware-about.md) improvements:

  * When a model fails a data test, state-aware orchestration rebuilds it on subsequent runs instead of reusing it from prior state to ensure dbt reevaluates data quality issues.
  * State-aware orchestration now detects and rebuilds models whose tables are deleted from the warehouse, even when there are no code or data changes. Previously, tables deleted externally were not detected, and therefore not rebuilt, unless code or data had changed. For more information, review [Handling deleted tables](https://docs.getdbt.com/docs/deploy/state-aware-about.md#handling-deleted-tables).

  State-aware orchestration is in private preview. refer to the [prerequisites for using the feature](https://docs.getdbt.com/docs/deploy/state-aware-setup.md#prerequisites).

* **Enhancement:** [dbt dbt Wizard](https://docs.getdbt.com/docs/platform/wizard-platform.md) correctly detects column names across various `schema.yml` files, adds only missing descriptions, and preserves existing ones.

* **Enhancement**: The Fusion CLI now automatically reads environment variables from a `.env` file in your current working directory (the folder you `cd` into and run dbt commands from in your terminal), if one exists. This provides a simple way to manage credentials and configuration without hardcoding them in your `profiles.yml`. The [dbt VS Code extension](https://docs.getdbt.com/docs/about-dbt-extension.md) also supports `.env` files and LSP-powered features. For more information, refer to [Configure environment variables](https://docs.getdbt.com/docs/local/configure-environment-variables.md).

* **New**: The new Semantic Layer YAML specification creates an open standard for defining metrics and dimensions that works across multiple platforms. The new spec is now live in the dbt Fusion engine.

  Key changes:

  * Semantic models are now embedded within model YAML entries. This removes the need to manage YAML entries across multiple files.
  * Measures are now simple metrics.
  * Frequently used options are now top-level keys, reducing YAML nesting depth.

  For an overview of the changes and steps how to migrate to the latest YAML spec, check [Migrate to the latest YAML spec](https://docs.getdbt.com/docs/build/latest-metrics-spec.md).

* **Fix:** Debug logs in the **Run summary** tab are now properly truncated to improve performance and user interface responsiveness. Previously, debug logs were not truncated correctly, causing slower page loads. You can access the full debug logs by clicking **Download > Download all debug logs**. For more information, review [Run visibility](https://docs.getdbt.com/docs/deploy/run-visibility.md#run-summary-tab).

* **New:** The [Semantic Layer querying](https://docs.getdbt.com/docs/explore/navigate-dbt-insights.md#semantic-layer-querying) within dbt Insights is now generally available (GA), enabling you to build SQL queries against the Semantic Layer without writing SQL code.

* **Enhancement**: Eligible dbt platform accounts in the Fusion private preview can now use [Exposures](https://docs.getdbt.com/docs/platform-integrations/downstream-exposures.md).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
