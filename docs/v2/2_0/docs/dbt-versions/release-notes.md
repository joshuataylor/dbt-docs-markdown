# dbt release notes

dbt platform

dbt release notes for recent and historical changes. Release notes fall into one of the following categories:

* **New:** New products and features
* **Enhancement:** Performance improvements and feature enhancements
* **Fix:** Bug and security fixes
* **Behavior change:** A change to existing behavior that doesn't fit into the other categories, such as feature deprecations or changes to default settings

Release notes are grouped by month for both multi-tenant and virtual private cloud (VPC) environments. ![RSS](/img/fontawesome/rss.svg)Subscribe to release note updates via [RSS](https://docs.getdbt.com/assets/files/release-notes-rss-b51b74f14de7e2bfe6beadbc9a78968f.xml), [Atom](https://docs.getdbt.com/assets/files/release-notes-atom-bf07c87791839c24a720fc04df230f95.xml), or [JSON Feed](https://docs.getdbt.com/assets/files/release-notes-rss-ebea384ace46694f733ee6e094e4feff.json).

For dbt Fusion engine updates, refer to the [dbt-fusion changelog](https://github.com/dbt-labs/dbt-core/blob/main/CHANGELOG-fusion.md).

## August 2026

* **Enhancement:** [dbt State](../deploy/dbt-state-about.md) now reuses views that only use `select *` on CTEs. Previously, any `select *` anywhere in a view caused a rebuild. Views that use `select *` directly on a `ref()` or `source()` still force a rebuild, because dbt cannot safely determine the output columns at parse time. For more information, refer to [Views with `select *`](../../faqs/State/views-rebuilt.md#views-with-select).
* **Enhancement:** dbt State now fetches table metadata in the background at the start of each run, so execution doesn't stall. Any node that is ready to skip or clone proceeds immediately, without waiting for the fetch to complete. Previously, dbt waited for the entire metadata fetch to complete before any node could execute. For more details, refer to [About dbt State](../deploy/dbt-state-about.md).
* **Enhancement:** On Snowflake, when [`metadata_warehouse`](../../reference/resource-configs/metadata-warehouse.md) is configured, dbt State now issues multiple, individual queries (one per schema) in parallel on the dedicated warehouse — faster than the single, consolidated query dbt runs by default. Without a dedicated warehouse, dbt now emits a warning if the metadata fetch takes longer than 15 seconds.
* **New:** The [`allow_clones`](../../reference/resource-configs/allow-clones.md) profile-level setting lets you control whether dbt State can clone tables into a target environment. Previously, there was no way to disable cloning — dbt State always cloned into any environment when a matching table was found.
* **New**: [`compare_unrendered_code`](../../reference/resource-configs/compare-unrendered-code.md) is a new dbt State config that checks the Jinja template for unrendered code changes. If dbt detects unrendered code changes, it then compares the rendered SQL. A rebuild only occurs when *both* have changed. This prevents unnecessary rebuilds for nodes that use non-deterministic macros or environment variables.
* **New:** When dbt State is enabled, you can run `dbt state explain` (dbt Core 2.0) or `dbt-state explain` (dbt Core plugin) in the CLI after a job finishes to see why dbt State made each decision and whether each node was built, reused, or cloned. For a detailed breakdown, run the command with `--verbose -s my_node_name` to see the table analysis, query analysis, and data freshness analysis for a specific node. For more information, refer to [`dbt state explain`](../../reference/commands/state-explain.md).
* **Enhancement:** New sessions open on the Wizard tab when available, and the Studio IDE remembers your last-used tab for each project so you can pick up where you left off.
* **Enhancement:** A new `relationName` field on the `ModelAppliedStateNode` and `ModelAppliedStateNestedNode` GraphQL types exposes the fully-qualified, adapter-rendered relation name (for example, `"database"."schema"."model_name"`) from the last successful model build.
* **New:** When dbt State is enabled, you can run `dbt state explain` (dbt Core 2.0) or `dbt-state explain` (dbt Core plugin) in the CLI after a job finishes to see why dbt State made each decision and whether each node was built, reused, or cloned. For a detailed breakdown, run the command with `--verbose -s my_node_name` to see the table analysis, query analysis, and data freshness analysis for a specific node. For more information, refer to [`dbt state explain`](../../reference/commands/state-explain.md).
* **Enhancement:** New sessions open on the Wizard tab when available, and the Studio IDE remembers your last-used tab for each project so you can pick up where you left off.
* **Enhancement:** A new `relationName` field on the `ModelAppliedStateNode` and `ModelAppliedStateNestedNode` GraphQL types exposes the fully-qualified, adapter-rendered relation name (for example, `"database"."schema"."model_name"`) from the last successful model build.
* **Beta**: [dbt Core 2.0](./dbt-upgrade/upgrading-to-v2.md) is now available in beta!
* **New:** The [Analyst read](../platform/manage-access/enterprise-permissions.md#analyst-read) permission set is now generally available (GA) for Enterprise plans. Analyst read is a project-level permission set that provides read-only access to analyze dbt models and project resources, and read-only users can connect to analysis features such as the [dbt MCP server](../dbt-ai/about-mcp.md).
* **Enhancement:** [Cost Insights](../explore/cost-insights.md) now supports cost attribution for [Snowflake Adaptive Warehouses](https://docs.snowflake.com/en/user-guide/warehouses-adaptive). For setup details, refer to [Assign required permissions](../explore/set-up-cost-insights.md#assign-required-permissions).
* **New:** The [Model timing tab](../deploy/run-visibility.md#model-timing-tab) in job run details has been redesigned with a richer, scalable view that includes metric tiles, an execution timeline with grouping and highlight controls, a concurrency-over-time chart, and a searchable resource details table.
* **New:** Semantic Layer development connections to Redshift now support external OAuth using Okta or Microsoft Entra with AWS IAM Identity Center.
* **Enhancement:** System for Cross-domain Identity Management (SCIM) API errors for seat or licensing failures now include email addresses so you can identify which users are blocking provisioning.
* **Behavior change:** Semantic Layer GraphQL queries that exceed the complexity limit of 200,000 now return an error instead of completing with a warning. If you hit this error, request fewer fields, use pagination, narrow your filters, or split the query into smaller ones.
* **Enhancement:** The [Analyst read](../platform/manage-access/enterprise-permissions.md#analyst-read) permission set now includes read access to **Connections** (account and project), **Projects**, Git repository settings, Semantic Layer configuration, **Environments**, custom environment variables, and Catalog metadata. Analysts can view that configuration without assigning additional permission sets, once added to a group with Analyst read.

## July 2026

* **Enhancement:** The [dbt State usage page](../deploy/dbt-state-interface.md) now shows daily active target tables (DATTs) split into **Billable** and **Free**. During a trial, all DATTs are counted as free.

* **Preview**: [The dbt Wizard home tab in dbt platform](../platform/wizard-home.md) is now available in public preview. You can build and change dbt projects through natural language, with inline diffs, DAG previews, and validation built in.

* **New:** The [dbt MCP server](../dbt-ai/mcp-available-tools.md#discovery) now uses one `get_node_details` tool for all resource types. The older type-specific tools are deprecated and will be removed in a future release.

* **New:** When a job is deactivated, the banner now shows a specific reason (repeated run failures, account inactivity, or a generic fallback) with tailored reactivation instructions for each case.

* **Enhancement:** The agent now automatically retries transient LLM provider failures (network timeouts, rate limits, and server errors) with exponential backoff, so brief provider blips are less likely to surface as errors during your session.

* **Enhancement:** Client tool loops now run until the agent finishes rather than stopping after 50 iterations, eliminating premature termination of long-running agentic workflows.

* **Enhancement:** Code blocks in dbt Wizard responses now include a Copy button on hover, so you can reuse generated SQL or YAML more easily.

* **Enhancement:** When a Bring-Your-Own-Key (BYOK) OpenAI model is configured with a deployment that does not support embeddings (for example, a `gpt-4o` Azure deployment), the similar models feature now returns an actionable error message prompting you to use a text-embedding model instead of a generic internal error.

* **Enhancement:** The `get_lineage` tool now accepts a `direction` parameter (`upstream`, `downstream`, or `both`) to narrow results to only ancestors or only descendants of a target node, reducing response size for large graphs. The response also now includes a `description` field on each returned node.

* **Enhancement:** The `list_metrics` tool now accepts a `meta_filter` parameter to restrict results to metrics whose `config.meta` contains specified key-value pairs (for example, `{"agent_accessible": true}`), keeping result sets small enough to preserve description and metadata in the response.

* **Enhancement:** The `ModelAppliedFilter` input type now includes a `health` field, letting you filter applied models by health status (`unknown`, `degraded`, `caution`, or `healthy`) directly in the Discovery API.

* **Enhancement:** The `RunStatus` enum and the `lastRunStatus` field on model execution information now include `warn`, so models whose last run completed with warnings correctly reflect that status.

* **Enhancement:** When searching from within a project environment route (for example, a Staging page), the Catalog search now defaults the environment filter to that environment type rather than always defaulting to Production.

* **Enhancement:** A redesigned search result card replaces tooltip-based match pills with inline expandable snippets for columns, tags, descriptions, and code matches. Please contact your account manager to enable.

* **Enhancement:** The `latest-fusion` release track is now Fusion Stable across all settings. Existing configurations have been updated automatically. No action is needed.

* **Enhancement:** On the Enable Fusion Environments page, environments already running Fusion now show a disabled checkbox, preventing unnecessary saves.

* **Enhancement:** When saving a Fusion upgrade fails, the platform now displays the top-level user message from the API instead of internal field-level error details.

* **Enhancement:** The command panel now shows live status updates as commands run, so you see progress sooner without waiting for a refresh.

* **Enhancement:** The "Committed spend" card is now labeled "Consumption pool" with copy explaining that usage-based features like dbt State draw from it. The card now appears between the current plan metric tiles and the product-specific sections on billing Overview and usage tab pages.

* **Enhancement:** The Daily Active Target Tables (DATTs) chart now stacks billable and free series, so trial users whose usage is entirely free see real bars instead of an empty chart.

* **Enhancement:** Memory-tuning optimizations are now applied automatically to all Fusion runs, reducing out-of-memory kill rates and improving overall uptime.

* **Fix:** Claude-backed agents can return longer answers and handle some previously broken interactions more reliably.

* **New**: [Apache Ossie](https://github.com/apache/ossie) semantic layer support:

  * Open Semantic Interchange (OSI) has been renamed to Apache Ossie. For more information, refer to [OSI is now Apache Ossie (Incubating)](https://www.getdbt.com/blog/osi-is-now-apache-ossie).
  * dbt writes an `osi_document.json` file to your `target/` directory alongside `semantic_manifest.json` at parse time. This artifact provides an Ossie representation of your project's Semantic Layer. For more information, refer to [Semantic manifest](../../reference/artifacts/sl-manifest.md#apache-ossie-document).
  * dbt supports the Ossie standard for defining semantic models and metrics. You can place Ossie-format `.json` files in an `osi/` directory at the root of your project, and dbt parses them into the manifest alongside any native dbt semantic models. To use a different directory, configure [`osi-paths`](../../reference/project-configs/osi-paths.md) in `dbt_project.yml`. Ossie versions `0.1.0` and `0.1.1` are supported; any other version raises a parse error. For more information, refer to [Ossie semantic layer documents](../build/ossie-semantic-models.md).

* **New:** [Cost Insights](../explore/cost-insights.md) is now generally available (GA) for Snowflake, BigQuery, and Databricks.

* **Preview:** [Cost Insights](../explore/cost-insights.md) for Amazon Redshift is now in preview.

* **Enhancement:** Users with `user_credential_write` access can now view and manage their credentials without needing `credentials_read` privileges. This update reduces the need for additional, broader permissions when performing credential updates.

* **Enhancement:** The [dbt Wizard](../platform/wizard-platform.md) in dbt platform has a redesigned empty state with updated suggested prompts to help you discover different ways to get started. A new wayfinder bar keeps your current project and branch visible and highlights the next step as you move from asking questions to changing code and opening a pull request.

* **Enhancement:** Catalog now supports a **Warn** last-run status. Resources whose last run completed with warnings show a distinct status and tooltip, and you can filter by **Warn** alongside other run statuses.

* **New:** You can now create hybrid jobs to track runs triggered by an external orchestrator. Hybrid jobs have a simplified setup that omits execution steps, triggers, advanced settings, and cost-optimization controls. They display **Externally triggered** as their next-run schedule and are available only for projects configured as [Hybrid projects](../deploy/hybrid-projects.md).

* **Enhancement:** Runs using a Fusion dbt version now invoke the built-in [`dbt lint`](../../reference/commands/lint.md?version=2.0) command instead of SQLFluff. Fusion virtual environments do not include SQLFluff, so linting now works for all Fusion-version runs and runs faster.

* **Enhancement:** When the agent compresses conversation context in the background, a spinner labeled **Optimizing conversation context…** now appears in the chat area. Submitting new messages and stopping the agent are disabled while compaction is in progress to prevent conflicts.

* **Enhancement:** When [dbt Wizard](../platform/wizard-platform.md) is unavailable (not activated, trial expired, or spend limit reached), Studio IDE now shows a dedicated screen with the specific reason and an appropriate action instead of a generic message.

* **Enhancement:** The users table, group member lists, and user edit drawer now search, filter, sort, and paginate server-side. On large accounts, all users are findable by name, email, or license type, group member search no longer misses results beyond the first page, and users beyond the first page can be opened and edited in the user edit drawer.

* **Enhancement:** The **Enable global account discovery** setting on the **Account settings** page is now visible to all entitled accounts without requiring a feature flag. You can allow or restrict account discovery from [Account settings](../platform/account-settings.md#enable-global-account-discovery).

* **Enhancement:** Credential-level [connection overrides](../dbt-platform-environments.md#extended-attributes) (such as Databricks catalog, Snowflake warehouse, role, and database) are now surfaced as a read-only **Connection overrides** section in the profile details view, without requiring you to open the edit form.

* **Fix:** When a run pod is Out of Memory (OOM)-killed and restarted, the platform now passes the correct status code and message to the config API so the run transitions to a failed state in the dbt platform UI instead of remaining **running** indefinitely.

* **Fix:** The Secure Shell (SSH) connection and authentication timeouts for Semantic Layer data platform connections are now 30 seconds (previously 1 second). If your bastion host or network path has higher latency, you will no longer experience deterministic connection failures. Refer to [Set up the Semantic Layer](../use-dbt-semantic-layer/setup-sl.md) for more information.

* **Fix:** Some types of Compile SQL queries are now rejected if they are too complex. If a request fails with a validation error, try reducing the number of metrics or group-by dimensions in the query.

* **Behavior change:** You can no longer create a [service token](../dbt-apis/service-tokens.md) using an account-scoped [personal access token](../dbt-apis/user-tokens.md) (PAT). Requests to the service tokens endpoint authenticated with a PAT now return a `400` error. Use a service token to create new ones instead.

* **New:** You can now access dbt State settings from **Account settings** > **Billing & Usage**, previously found under **State**. You can manage your trial, enable dbt State on environments and jobs, and set spend alerts — all in one place. For details, refer to [dbt State trial and billing](../deploy/dbt-state-trial.md).

* **New:** Redshift development connections now support [external OAuth](../platform/manage-access/redshift-external-oauth.md) (Okta or Entra ID) through AWS IAM Identity Center.

#### Docs changes

To simplify the docs experience, clarify availability, and make it easier to find what applies to you, we made the following changes to the docs site:

*tl;dr:* The docs are now organized around v1 and v2 for simplified docs versioning and navigation. We've clarified dbt Core and licensing, reorganized v2 content, and refreshed adapter and Fusion availability guidance. If you notice anything off or have any feedback, we'd love to hear it! Open up a [docs issue here](https://github.com/dbt-labs/docs.getdbt.com/issues).

* **Enhancement**: We've updated the version switcher on the docs site. The version switcher now just shows v1 and v2. v2 is the current generation of dbt, built on Rust for a faster, richer dev experience; v1 is the Python-based generation of dbt. Refer to [dbt versions](../introduction.md#dbt-versions) for what's different between v1 and v2.
* **New:** We've added a dedicated page explaining dbt Core and its distributions. dbt Core 2.0 is the Rust-based open-source runtime. dbt Core v1.x is the Python-based runtime. Refer to [About dbt Core](../introduction.md) for more info.
* **New:** Licensing across dbt Core now has its own page, so you can see what applies to your setup in one place. Refer to [dbt licensing](../dbt-licensing.md).
* **Enhancement:** [Static analysis](../build/about-static-analysis.md) now lives with the rest of your build docs and available in v2.
* **Enhancement:** The Fusion upgrade readiness checklist now sits right next to the [v2 upgrade guide](./dbt-upgrade/upgrading-to-v2.md), and the networking and telemetry references moved in [local install](../local/dbt-networking-requirements.md) and [Reference](../../reference/telemetry-observability.md).
* **Enhancement:** More adapters are closer to general availability — Snowflake, BigQuery, Databricks, and Redshift are now in **Preview**, and Spark and DuckDB are in **Beta**. Refer to [Adapter lifecycles](../dbt/dbt-availability.md?version=2.0#adapter-lifecycle) for the current status of each adapter.
* **Enhancement:** Simplified and clarified the [Fusion feature tables](../dbt/dbt-availability.md?version=2.0#what-you-get-with-fusion) to make it easier to see what's available and how to get it.
* **New:** Added availability badges to pages and sections so you can quickly see what applies to your setup at a glance.

## June 2026

* **Enhancement:** [Column-level tags](../../reference/resource-configs/tags.md) defined in your dbt project now appear on the **Columns** tab of resource details pages in Catalog. You can click any tag badge to filter the lineage view, or search for columns directly by tag name. Refer to [View resource details](../explore/explore-projects.md#view-resource-details).
* **Enhancement:** You can now enable [dbt State](../deploy/dbt-state-about.md) on continuous integration and merge job types, in addition to deploy jobs. For more information, refer to [Enabling dbt State on individual jobs](../deploy/dbt-state-enable-jobs.md).
* **Enhancement**: The [Cost Insights](../explore/cost-insights.md) table view now includes **All** and **Jobs** buttons to switch between an aggregated cost view and a per-job cost breakdown. Available in the project dashboard and the **Model performance** section in Catalog. When **Jobs** is selected, the CSV export includes job-level data. For more information, refer to [Explore cost data](../explore/explore-cost-data.md).
* **Enhancement:** [dbt Wizard](../platform/wizard-platform.md) tool calls for dbt command invocations now stream their output live in chat, in both the Studio IDE and [Wizard home](../platform/wizard-home.md).
* **Enhancement:** You can now download files from the Studio IDE File explorer. Right-click a file and select **Download** to save it to your computer. For more information, refer to the [Studio IDE user interface](../platform/studio-ide/ide-user-interface.md#basic-layout).
* **Fix:** If you use the Administrator API to manage [SCIM](../platform/manage-access/scim.md) to sync users from your identity provider, the `/api/v3/accounts/{account_id}/scim/v2/Users` response now returns `value` and `display` on each embedded group reference. `id` and `displayName` are retained so existing integrations keep working — this is a non-breaking change.
* **Enhancement**: The [Administrative API v3](https://docs.getdbt.com/dbt-cloud/api-v3) now supports private endpoint operations — [`list`](https://docs.getdbt.com/dbt-cloud/api-v3?version=2.0#/operations/List%20Private%20Endpoints), [`create`](https://docs.getdbt.com/dbt-cloud/api-v3?version=2.0#/operations/Create%20Private%20Endpoint), [`retrieve`](https://docs.getdbt.com/dbt-cloud/api-v3?version=2.0#/operations/Retrieve%20Private%20Endpoint), [`update`](https://docs.getdbt.com/dbt-cloud/api-v3?version=2.0#/operations/Update%20Private%20Endpoint), and [`delete`](https://docs.getdbt.com/dbt-cloud/api-v3?version=2.0#/operations/Delete%20Private%20Endpoint). Use these endpoints to manage private connectivity programmatically.
* **Enhancement**: You can [download OpenTelemetry (OTel) logs](../deploy/run-visibility.md#access-logs) for individual dbt command steps in Fusion job runs.
* **Enhancement**: You can now configure [dbt State](../deploy/dbt-state-about.md) for the Studio IDE directly in the dbt platform UI — either as a team-wide default on your development environment, or as a personal override. For more information, refer to [Enabling dbt State in Studio](../deploy/dbt-state-enable-studio.md).
* **New:** [Model query history](../explore/model-query-history.md) for Redshift and Databricks is now generally available (GA).
* **Behavior change:** On September 1, 2026, several behavior change flags on the dbt platform **Latest** release track will reach maturity (enabled by default). Refer to [Flags reaching maturity](../../reference/global-configs/behavior-changes.md#flags-reaching-maturity) to see which flags may affect your project and how to opt out before then.
* **Beta:** The dbt Fusion engine now supports the Salesforce Data 360 connection in the dbt platform. For more information, refer to [Connect Salesforce Data 360](../platform/connect-data-platform/connect-salesforce.md).
* **Private beta**: The [Analyst read](../platform/manage-access/enterprise-permissions.md#analyst-read) permission set is available for Enterprise plans.
  * Analyst read is a project-level permission set that provides read-only access to analyze dbt models and project resources. The OAuth integration that lets read-only users connect to analysis features (such as the [dbt MCP server](../dbt-ai/about-mcp.md)) is available to use, while the Analyst read permission set and read-only permission changes are in private beta. To enable them, contact your account manager.
* **Beta**: Workspace-level Private Link for Microsoft Fabric is now available in beta. Configure a private connection between the dbt platform and your Fabric workspace so SQL traffic stays on Azure's private network. For more information, refer to [Configuring Private Link for Microsoft Fabric](../platform/secure/private-connectivity/azure/azure-fabric.md).
* **Beta**: [Cost Insights](../explore/cost-insights.md) now supports Amazon Redshift Serverless and provisioned clusters. Configure your platform metadata credentials with the `sys:monitor` role or `SYSLOG ACCESS UNRESTRICTED` permission to allow dbt to read cross-user query history, then set your pricing in Cost Insights settings. For more information, refer to [Set up Cost Insights](../explore/set-up-cost-insights.md).

### Snowflake Summit 2026 announcements

The following features are new or enhanced as part of dbt Labs announcements at [Snowflake Summit 2026](https://www.getdbt.com/events/snowflake-summit-2026) in San Francisco from June 1–4, 2026:

* **Alpha**: [dbt Core 2.0](./dbt-upgrade/upgrading-to-v2.md) is now available in alpha!

  * **New**: dbt Core 2.0 is the open-source Apache 2.0 foundation that the dbt Fusion engine builds on, delivering a faster, Rust-based runtime. It ships as two distributions: `dbt-core` (OSS, Apache 2.0) and `dbt` (Fusion distribution, proprietary).

* **Beta**: [`dbt lint`](../../reference/commands/lint.md?version=2.0) is now available in beta!

  * **New**: `dbt lint` is a high-performance SQL linter built into the dbt platform, available on projects running the dbt Fusion engine. It is SQLFluff-compatible; it reads your existing `.sqlfluff` config, uses the same rule codes, and respects `-- noqa` suppression comments. In benchmarks, it runs roughly 50× faster than single-threaded SQLFluff..

* **Preview**: [dbt Docs v2](../build/view-documentation.md#dbt-docs-v2) is now available in preview!

  * **New**: dbt Docs v2 is a next-generation open-source catalog experience available with the dbt Fusion engine and dbt Core 2.0. It uses a compact binary index instead of loading the full `manifest.json` in the browser, making it significantly faster for large projects.
  * **New**: dbt Docs v2 includes a redesigned UI, Semantic Layer metadata, column-level lineage (Fusion only), and a REST API at `/api/v1/` so AI agents and MCP servers can query your dbt project metadata without a browser.
  * **New**: Generate and serve [dbt Docs v2](../build/view-documentation.md#dbt-docs-v2) with the dbt Fusion engine or dbt Core 2.0 by running a dbt command with `--use-index`, then `dbt docs serve`. Add [`--write-catalog`](../../reference/commands/cmd-docs.md#--write-catalog-flag) for richer column type metadata.

* **Preview**: [dbt State](../deploy/dbt-state-about.md) is now available in preview!

  * **New**: dbt State skips or clones nodes when the logic and data haven't changed, rather than rebuilding everything on every run. Available natively in dbt v2.0, the dbt platform, and the dbt Fusion engine, and as a plugin for dbt Core v1.7-1.12. To get started, refer to [Set up dbt State](../deploy/dbt-state-setup.md).
  * **New**: [dbt State pricing](../platform/billing/dbt-state-usage.md) is usage-based at $0.094 per daily unique reuse. New organizations receive a 30-day free trial with no usage limit.
  * **Behavior change**: State-aware orchestration is no longer being enabled for new customers. Refer to [Migrate to dbt State](../deploy/dbt-state-migration.md) for more information.

* **New**: dbt Wizard is available in dbt platform as a public preview. Introducing dbt Wizard CLI as a public beta. Purpose-built for agentic governed data development in dbt, dbt Wizard understands your project through a [native metadata engine](../dbt-ai/wizard-how-it-works.md#native-metadata-engine), unlike general-purpose coding agents.

  * **New**: [Support for Anthropic as a BYOK provider for dbt AI](../platform/enable-dbt-ai.md#configure-your-ai-provider).
  * **New**: [`dbt login`](../../reference/commands/login.md?version=2.0) is a new CLI command available in dbt Core 2.0 and later. It opens browser-based authentication and shares your login state across the CLI, dbt VS Code extension, dbt State, and dbt Wizard CLI with no separate sign-in flows needed.

* **New:** OAuth client registrations now accept custom-scheme redirect URIs (for example, `cursor://` or `vscode://`), so you can build native app OAuth integrations with Cursor and VS Code.

* **New:** Public REST API endpoints at `/api/ide/v3/{environment_id}/files/` support Studio IDE workspace file operations, including stat, read, write, list, delete, mkdir, and rename. Pass file paths as query parameters.

* **New:** The `GET /api/ide/v3/{environment_id}/status` endpoint returns the `dbt_version` and `is_fusion` status for a given environment.

* **New:** The dbt platform CLI Python client's `create_invocation()` method now supports a `workspace` parameter, so you can run invocations against persisted workspace files on workers.

## May 2026

* **Enhancement:** Repository clone failures now surface a more actionable diagnostic message to help you resolve common issues faster. For guidance, refer to [Troubleshooting clone errors](../platform/git/import-a-project-by-git-url.md#troubleshooting-clone-errors).
* **Fix:** The connection test failure message now prompts you to verify your connection details and confirm that your credentials have access to the data warehouse, rather than showing a generic failure message.
* **Enhancement:** Users granted `user_credential_write` can access **Your profile** > **Credentials** without `develop_access` (including read-only users). Environment variable overrides and dbt version overrides still require `develop_access`. Refer to [Enterprise permissions](../platform/manage-access/enterprise-permissions.md) for more information.
* **New:** The [Job creator permission set](../platform/manage-access/enterprise-permissions.md#job-creator) is now available for Enterprise accounts. Assign it to users who need to create, edit, and run jobs within assigned projects and environments without access to edit environments or environment variables.
* **Enhancement:** The admin API toolset (job management and run operations) is now always available in the dbt and no longer requires a feature flag. You no longer need to contact your account manager to enable these tools.
* **Fix:** When a job cannot clone its repository because no remote URL is configured, the error message now explains the most likely causes (an invalid Git remote URL, a Git provider outage, or a deprecated HTTPS connection) and directs you to verify the URL, confirm your provider is operational, and ensure the repository uses SSH with deploy keys before retrying.
* **New:** The **Notification Manager** [permission set](../platform/manage-access/enterprise-permissions.md) is now available for Enterprise accounts. Assign it to users who need to manage Slack, Microsoft Teams, and email job notifications across all projects without requiring full Account Admin access.
* **Beta**: [Cost Insights](../explore/cost-insights.md), available in public beta, shows estimated warehouse compute costs and run times for dbt projects and models in dbt platform, highlighting efficiency gains from [state-aware orchestration](../deploy/state-aware-about.md). Refer to [Set up Cost Insights](../explore/set-up-cost-insights.md) and [Explore cost data](../explore/explore-cost-data.md) to learn more.
* **New:** Fusion release tracks are now being rolled out across accounts in phases. Refer to [Fusion release tracks](./dbt-release-tracks.md?version=2.0#fusion-release-tracks) for more information.
* **Enhancement:** Commands run by and the now appear in the Studio IDE **Commands** tab with a icon and **Run by Copilot** tooltip, so you can tell agent-run commands apart from manually run ones.
* **Fix:** [`state:modified`](../../reference/node-selection/methods.md#state) now detects changes to [UDF](../build/udfs.md) properties (such as `arguments` and `returns`) defined in `.yml` files. Previously, only changes to the SQL or Python function body were detected.
* **New:** [Native private packages](../build/packages.md#native-private-packages) are now generally available (GA).
* **Preview**: The [Developer agent](../dbt-ai/wizard-ide.md) is now in preview. Use natural language prompts to build or refactor models, and generate SQL, tests, documentation, and semantic models from scratch. For more information, refer to the [Developer agent](../dbt-ai/wizard-ide.md).
* **Behavior change:** When you set up single sign-on (SSO) in the dbt platform, the SSO slug is now system-generated and read-only. Existing SSO configurations remain valid, but you can’t change the slug. If you delete and recreate your SSO configuration, the new configuration uses a new, system-generated slug. Refer to [Single sign-on overview](../platform/manage-access/sso-overview.md) for more information.
* **Enhancement:** The [dbt VS Code extension](../install-dbt-extension.md?version=2.0) now supports account creation. If you sign in with an existing dbt user that doesn't have an associated dbt platform account, the registration flow prompts you to create one instead of requiring a separate workflow.
* **Enhancement:** Delete individual [dbt Wizard chat conversations](../dbt-ai/wizard-ide.md#availability-and-considerations) from the conversation list (three dots → **Delete**). Deleting the open conversation clears the panel.
* **New:** The Fusion + Snowflake connection experience is now generally available on the dbt platform. See our [Fusion upgrade guides](../../guides/prepare-dbt-upgrade.md?step=1) for information on enabling the upgrade workflows for your environments today!
* **Enhancement:** In the Discovery API [Tests object schema](../dbt-apis/discovery-schema-environment-applied-tests.md), you can now filter `environment.applied.tests` by multiple test result statuses in a single query using the new `lastKnownResults: [TestStatus]` filter field on `TestAppliedFilter`. The single-value `lastKnownResult` filter field is still supported but deprecated. Update your queries to use `lastKnownResults` going forward.
* **Enhanced** Fusion eligibility job prompts now use a **Debug on Fusion** dropdown instead of a standalone **Run once on Fusion** button. For more information, refer to [Update your jobs](../../guides/prepare-dbt-upgrade.md?step=7).
* **Enhancement:** The input bar now supports arrow key history navigation. Press the up arrow at the start of the input to cycle through previous inputs, and the down arrow at the end to return to more recent ones. dbt stores up to 5 previous inputs per session.
* **Enhancement:** Tool approval and file edit dialogs in the now support number key shortcuts (1, 2, 3) to select options. The first option is auto-focused when a dialog appears, so you can act immediately without clicking.

## April 2026

* **Enhancement:** When a dbt command run by the times out, the agent now automatically attempts to cancel the stuck invocation on the server and returns a retry-friendly message, letting you decide whether to retry. Previously, timeouts resulted in an unhandled error. This applies to both model invocations and autofix runs.
* **Enhancement:** In dbt platform run logs, `dbt ls` and `dbt list` now display node results as **No-op** instead of **Unknown** when using dbt Fusion engine. Refer to [dbt ls (list)](../../reference/commands/list.md) for more information.
* **New:** A universal login URL is available at <https://login.dbt.com>, making it easier for you to view accounts you have access to across instances (regions and tenancies). This is currently available for multi-tenant accounts with an account-specific domain, and support for single-tenant accounts is coming soon. For more information, refer to [Log in to dbt platform](../platform/about-platform/login.md).
* **Fix:** Refreshing the same browser tab now restores your active dbt Wizard conversation instead of showing the empty state. Opening a new tab, or returning after closing the tab, still starts in the empty state. The dbt Wizard is currently in beta.
* **Enhancement:** The dbt VS Code extension's **Get started** panel has been redesigned and surfaces the exact next setup step you need to install the extension and Fusion. The new panel also supports a new **agentic migration** option that helps you upgrade your project to Fusion automatically in Copilot or Cursor. For more info, see [Getting started](../install-dbt-extension.md#getting-started).
* **Beta**: [Model query history](../explore/model-query-history.md) now also supports Databricks and Redshift. Refer to [Credential permissions](../explore/model-query-history.md#credential-permissions) for more information.
* **Enhancement:** [Slack notifications (account-level)](../deploy/job-notifications.md#slack-notifications-account) and [Microsoft Teams notifications](../deploy/job-notifications.md#microsoft-teams-notifications) are now generally available, enabling you to send job notifications directly to Slack channels configured at the account level, and to Teams channels.
* **Enhancement:** When using the [dbt autofix](https://github.com/dbt-labs/dbt-autofix) tool in the Studio IDE, you can now compile your project directly from the results panel after a successful `dbt parse`. Click **Compile** next to the **Successfully resolved** result to kick off a compile. For more information, refer to [Fix deprecation warnings](../platform/studio-ide/autofix-deprecations.md).
* **Beta**: DuckDB is now supported in the dbt Fusion engine CLI, which lets you run local dbt projects without a warehouse account. For more information, refer to [Connect DuckDB](../local/connect-data-platform/duckdb-setup.md).
* **New**: You can now configure Snowflake PrivateLink endpoints directly in dbt platform without contacting dbt Support, available in private beta. Go to **Account settings → Integrations → Private endpoints** to request and manage Snowflake PrivateLink endpoints on AWS. This feature is available for Snowflake on AWS only. For more information, refer to [AWS PrivateLink for Snowflake](../platform/secure/private-connectivity/aws/aws-snowflake.md?version=1.12).
* **Enhancement:** You can now use arrays as values for keys in the dbt platform extended attributes YAML editor. For example, `db_groups: [db_editor, db_viewer]` is now valid. Previously, array values were only supported using the API. For more information, refer to [Extended attributes](../dbt-platform-environments.md#extended-attributes).
* **Beta**: The Redshift adapter now supports a `datasharing` profile credential on the dbt platform **Latest** release track. When set to `true`, dbt uses Redshift's native `SHOW` commands (for example, `SHOW TABLES`, `SHOW COLUMNS`, `SHOW SCHEMAS`) for metadata queries instead of PostgreSQL catalog tables, enabling cross-database and cross-cluster access with [Redshift Datasharing](https://docs.aws.amazon.com/redshift/latest/dg/datashare-overview.html). For more information, refer to [Redshift setup](../local/connect-data-platform/redshift-setup.md#datasharing).
* **Enhancement:** When a connection does not have platform metadata credentials configured yet, the credentials form now renders in edit mode immediately — you no longer need to click **Add credentials** first. If you cancel, the **Add credentials** button appears so you can return to the form. Existing connections with configured platform metadata credentials are unaffected. Refer to [Configure the warehouse connection](../explore/external-metadata-ingestion.md#configure-the-warehouse-connection) for more information.
* **New**: The [dbt Remote dbt MCP server](../dbt-ai/about-mcp.md?version=2.0) now supports Admin API calls! This allows users to troubleshoot job-related errors in agents like Claude and Cursor.
* **New**: The [Developer agent](../dbt-ai/wizard-ide.md) is now in beta. Use the Developer agent to write or refactor dbt models from natural language, generate documentation, tests, semantic models, and SQL code from scratch, giving you the flexibility to modify or fix generated code. For more information, refer to the [Developer agent](../dbt-ai/wizard-ide.md).
* **Enhancement:** The Studio IDE now validates dbt YAML using Fusion aligned JSON Schema from [dbt-jsonschema](https://github.com/dbt-labs/dbt-jsonschema) across [dbt platform release tracks](./dbt-release-tracks.md), including for development environments on dbt Core. This improves autocomplete and structural feedback in the editor. Diagnostics can occasionally disagree with what your environment accepts; use dbt runs and previews as the source of truth. For context, review [Migrate to the latest YAML spec](../build/latest-metrics-spec.md) and [dbt YAML validation in Studio](../platform/studio-ide/develop-in-studio.md#dbt-yaml-validation). This will be a phased rollout starting the week of April 6th.
* **Enhancement:** The Studio IDE status bar now offers more control, more detailed information, and quicker access to settings for deferral, dbt version, and project status. For more information, refer to the [Studio IDE docs](../platform/studio-ide/ide-user-interface.md#the-command-and-status-bar). These updates roll out in phases to existing accounts starting April 6.
* **Enhancement:** In Snowflake **Private endpoints**, output validation errors now display inline beneath the text area (instead of as a page-level banner). The **Submit request** button is also disabled when the output is invalid (for example, empty, malformed JSON, or missing required fields).
* **Enhancement:** The Studio IDE now supports deep links to a specific console tab using the `?consoleTab=` query parameter. For example, append `?consoleTab=problems` to open Studio with the **Problems** tab pre-selected. The `problems` tab applies only when it is available for the current session.

## March 2026

* **Enhancement:** The environment [Connection profiles](../platform/about-profiles.md#environment-profiles-table) page has been updated. The profile name is now a clickable button that opens the view/edit drawer, the Connection column links to the connection details page in a new tab, and in edit mode a **swap icon** button lets you change the assigned profile. The previous ellipsis menu has been removed. For details, refer to [About profiles](../platform/about-profiles.md).
* **Beta:** Apache Spark is now supported in the dbt Fusion engine CLI, enabling faster compilation and execution for Spark-based dbt projects. Fusion currently supports only Apache Spark 3.0. For more information, refer to [Connect Apache Spark to Fusion](../local/connect-data-platform/spark-setup.md).
* **Enhancement:** [Cost Insights](../explore/cost-insights.md) charts now include an **Assets** filter (**Models** / **Tests** / **All**) on the **Cost**, **Usage**, **Query run time**, and **Builds** tabs. Use the dropdown on each chart to filter the data you want to view; your selection is stored per tab. The former **Model builds** tab is now labeled **Builds**. For more information, refer to [Explore cost data](../explore/explore-cost-data.md).
* **Enhancement:** [Deferral](../../reference/node-selection/defer.md) now supports [user-defined functions (UDFs)](../build/udfs.md). When you run a dbt command with `--defer` and `--state`, dbt resolves `function()` calls from the state manifest. This lets you run models that depend on UDFs without first building those UDFs in your current target.
* **Fix**: Status messages that exceed the 1024 character limit are now automatically truncated to prevent validation errors and run timeouts. Previously, long status messages could cause runs to fail with unhandled exceptions or result in lost status information. The system now logs when truncation occurs to help identify and optimize verbose status messages.
* **Fix:** Resolved an issue where [retrying failed runs](../deploy/retry-jobs.md) that were triggered from Git tags would use the wrong commit. Previously, when runs were triggered from Git tags instead of branches, the system would enter a detached HEAD state, causing retries to use the latest commit on HEAD rather than the original tagged commit. The fix now correctly preserves and uses the original Git tag reference when retrying runs, ensuring consistency between the initial run and any retries.
* **New**: The [dbt MCP server](../dbt-ai/about-mcp.md?version=2.0#product-docs) now includes product docs tools (`search_product_docs` and `get_product_doc_pages`) that let your AI assistant search and fetch pages from docs.getdbt.com in real time. Get responses grounded in the latest official dbt documentation rather than relying on training data or web searches, so you can stay in your development flow and trust the answers. This allows you to stay in your development flow and trust. These tools are enabled by default with no additional configuration. Restart your MCP server if you don't see the product docs tools in your MCP config. For more information, refer to [the dbt MCP repo](https://github.com/dbt-labs/dbt-mcp?tab=readme-ov-file#product-docs).
* **Enhancement**: The Model Timing tab displays an informative banner for dbt Fusion engine runs instead of the timing chart. The banner explains "Model timing is not yet available for Fusion runs" and provides context about threading differences. Non-Fusion runs continue to show the timing chart normally.
* **Behavior change**: [Snowflake plans to increase](https://docs.snowflake.com/en/release-notes/bcr-bundles/un-bundled/bcr-2118) the default column size for string and binary data types in September 2026. `dbt-snowflake` versions below v1.10.6 may fail to build certain incremental models when this change is deployed. [Assess impact and take any required actions](../../reference/resource-configs/snowflake-configs.md#assess-impact-and-required-actions).
* **New**: The new Semantic Layer YAML specification is now available on the dbt platform **Latest** release track. For an overview of the changes and steps how to migrate to the latest YAML spec, refer to [Migrate to the latest YAML spec](../build/latest-metrics-spec.md).
* **Behavior change:** New projects in trial, starter, or Enterprise accounts now default to **Fusion Stable** for all new environments with a supported adapter (Redshift, Snowflake, BigQuery, and Databricks). You can revert to another version by changing the dbt version in your [environment settings](../dbt-platform-environments.md#change-environment-settings).

## February 2026

* **New**: Advanced CI (dbt compare in orchestration) is now supported in the dbt Fusion engine. For more information, review [Advanced CI](../deploy/advanced-ci.md).

* **Beta**: The `dbt-salesforce` adapter available in the dbt Fusion engine CLI is now in beta. For more information, refer to [Salesforce Data 360 setup](../local/connect-data-platform/salesforce-data-cloud-setup.md).

* **Enhancement:** The Analyst permission now has the project-level access to read repositories. Review [Project access for project permissions](../platform/manage-access/enterprise-permissions.md#project-access-for-project-permissions) for more information.

* **Enhancement:** After a user accepts an email [invite](../platform/manage-access/invite-users.md) to access an [SSO-protected](../platform/manage-access/sso-overview.md) dbt platform account, the UI now prompts them to log in with SSO to complete the process. This replaces the previous "Joined successfully" message, helping avoid confusion when users accept an invite but do not complete the SSO login flow.

* **New:** [Profiles](../platform/about-profiles.md) let you define and manage connections, credentials, and attributes for deployment environments at the project level. dbt automatically creates profiles for existing projects and environments based on the current configurations, so you don't need to take any action. This is being rolled out in phases during the coming weeks.

* **New**: [Python UDFs](../build/udfs.md) are now supported and available in dbt Fusion engine when using Snowflake or BigQuery.

* **Enhancement:** Minor enhancements and UI updates to the Studio IDE, file explorer that replicate the VS Code IDE experience.

* **Enhancement:** Profile creation now displays specific validation error messages (such as "Profile keys cannot contain spaces or special characters") instead of generic error text, making it easier to identify and fix configuration issues.

* **Private beta**: [Cost Insights](../explore/cost-insights.md) shows estimated warehouse compute costs and run times for your dbt projects and models, directly in the dbt platform. It highlights cost reductions and efficiency gains from optimizations like [state-aware orchestration](../deploy/state-aware-about.md) across your project dashboard, model pages, and job details. Refer to [Set up Cost Insights](../explore/set-up-cost-insights.md) and [Explore cost data](../explore/explore-cost-data.md) to learn more.

* **New**: The [dbt Semantic Layer](../use-dbt-semantic-layer/dbt-sl.md) now supports [Omni](https://docs.omni.co/integrations/dbt/semantic-layer) as a partner integration. For more information, refer to [Available integrations](../platform-integrations/avail-sl-integrations.md).

* **Enhancement**: We clarified documentation for cumulative log size limits on run endpoints, originally introduced in [October 2025](./2025-release-notes.md#october-2025). When logs exceed the cumulative size limit, dbt omits them and displays a banner. No functional changes were made in February 2026. For more information, review [Run visibility](../deploy/run-visibility.md#log-size-limits).

* **New**: The `immutable_where` configuration is now supported for Snowflake dynamic tables. For more information, refer to [Snowflake configurations](../../reference/resource-configs/snowflake-configs.md#immutable-where).

* **Fix**: The user invite details now show more information in invite status, giving admins visibility into users who accepted an invite to an SSO-protected account but haven't yet logged in via SSO. Previously, these invites were hidden, making it appear as if the user hadn't been invited. The Invites endpoints of the dbt platform Admin v2 API now include these additional statuses:

  * `4` (PENDINGEMAIL\_VERIFICATION)
  * `5` (EMAIL\_VERIFIED\_SSO).

* **Enhancement**: Improved performance on Runs endpoint for Admin V2 API and run details in dbt platform when connecting with GCP.

## January 2026

* **Enhancement:** The `defer-env-id` setting for choosing which deployment environment to defer to is [now available](../platform/about-defer.md#configure-deferral-environment-id) in the Studio IDE. Previously, this configuration only worked for the dbt platform CLI

* **Beta:** The [Analyst agent](../explore/navigate-dbt-insights.md#dbt-copilot) in dbt Insights is now in beta.

  * dbt dbt Wizard's AI assistant in Insights now uses a dropdown menu to select between **Agent** and **Generate SQL**, replacing the previous tab interface.

* **Enhancement:** The [Studio IDE](../platform/studio-ide/ide-user-interface.md#search-your-project) now includes search and replace functionality and a command palette, enabling you to quickly find and replace text across your project, navigate files, jump to symbols, and run IDE configuration commands. This feature is being rolled out in phases and will become available to all dbt platform accounts by mid-February.

* **Enhancement:** [State-aware orchestration](../deploy/state-aware-about.md) improvements:

  * When a model fails a data test, state-aware orchestration rebuilds it on subsequent runs instead of reusing it from prior state to ensure dbt reevaluates data quality issues.
  * State-aware orchestration now detects and rebuilds models whose tables are deleted from the warehouse, even when there are no code or data changes. Previously, tables deleted externally were not detected, and therefore not rebuilt, unless code or data had changed. For more information, review [Handling deleted tables](../deploy/state-aware-about.md#handling-deleted-tables).

  State-aware orchestration is in private preview. refer to the [prerequisites for using the feature](../deploy/state-aware-setup.md#prerequisites).

* **Enhancement:** [dbt dbt Wizard](../platform/wizard-platform.md) correctly detects column names across various `schema.yml` files, adds only missing descriptions, and preserves existing ones.

* **Enhancement**: v2 now automatically reads environment variables from a `.env` file in your current working directory (the folder you `cd` into and run dbt commands from in your terminal), if one exists. This provides a simple way to manage credentials and configuration without hardcoding them in your `profiles.yml`. The [dbt VS Code extension](../about-dbt-extension.md) also supports `.env` files and LSP-powered features. For more information, refer to [Configure environment variables](../local/configure-environment-variables.md).

* **New**: The new Semantic Layer YAML specification creates an open standard for defining metrics and dimensions that works across multiple platforms. The new spec is now live in the dbt Fusion engine.

  Key changes:

  * Semantic models are now embedded within model YAML entries. This removes the need to manage YAML entries across multiple files.
  * Measures are now simple metrics.
  * Frequently used options are now top-level keys, reducing YAML nesting depth.

  For an overview of the changes and steps how to migrate to the latest YAML spec, check [Migrate to the latest YAML spec](../build/latest-metrics-spec.md).

* **Fix:** Debug logs in the **Run summary** tab are now properly truncated to improve performance and user interface responsiveness. Previously, debug logs were not truncated correctly, causing slower page loads. You can access the full debug logs by clicking **Download > Download all debug logs**. For more information, review [Run visibility](../deploy/run-visibility.md#run-summary-tab).

* **New:** The [Semantic Layer querying](../explore/navigate-dbt-insights.md#semantic-layer-querying) within dbt Insights is now generally available (GA), enabling you to build SQL queries against the Semantic Layer without writing SQL code.

* **Enhancement**: Eligible dbt platform accounts in the Fusion private preview can now use [Exposures](../platform-integrations/downstream-exposures.md).
