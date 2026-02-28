# Orchestrate downstream exposures [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Use dbt [Cloud job scheduler](https://docs.getdbt.com/docs/deploy/job-scheduler.md) to proactively refresh downstream exposures and the underlying data sources (extracts) that power your Tableau Workbooks.

Available in private beta

Orchestrating exposures is currently available in private beta to dbt Enterprise accounts. To join the beta, contact your account representative.

Orchestrating exposures integrates with [downstream exposures](https://docs.getdbt.com/docs/cloud-integrations/downstream-exposures-tableau.md) and uses your `dbt build` job to ensure that Tableau extracts are updated regularly.

Control the frequency of these refreshes by configuring environment variables in your dbt environment.

 Differences between visualizing and orchestrating downstream exposures

The following table summarizes the differences between visualizing and orchestrating downstream exposures:

| Info              | Set up and visualize downstream exposures                                      | Orchestrate downstream exposures [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles") |
| ----------------- | ------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Purpose           | Automatically brings downstream assets into your dbt lineage.                  | Proactively refreshes the underlying data sources during scheduled dbt jobs.                                                                                               |
| Benefits          | Provides visibility into data flow and dependencies.                           | Ensures BI tools always have up-to-date data without manual intervention.                                                                                                  |
| Location          | Exposed in [Catalog](https://docs.getdbt.com/docs/explore/explore-projects.md) | Exposed in [dbt scheduler](https://docs.getdbt.com/docs/deploy/deployments.md)                                                                                             |
| Supported BI tool | Tableau                                                                        | Tableau                                                                                                                                                                    |
| Use case          | Helps users understand how models are used and reduces incidents.              | Optimizes timeliness and reduces costs by running models when needed.                                                                                                      |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

To orchestrate downstream exposures, you should meet the following:

* [Configured downstream exposures](https://docs.getdbt.com/docs/cloud-integrations/downstream-exposures-tableau.md) and ensured desired exposures are included in your lineage.

* Verified your environment and jobs are on a supported dbt [release track](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md).

* Have a dbt account on the [Enterprise or Enterprise+ plan](https://www.getdbt.com/pricing/).

* Created a [production](https://docs.getdbt.com/docs/deploy/deploy-environments.md#set-as-production-environment) deployment environment for each project you want to explore, with at least one successful job run.

* Have [admin permissions](https://docs.getdbt.com/docs/cloud/manage-access/enterprise-permissions.md) in dbt to edit project settings or production environment settings.

* Configured a [Tableau personal access token (PAT)](https://help.tableau.com/current/server/en-us/security_personal_access_tokens.htm) whose creator has privileges to view and refresh the data sources used by your exposures. The PAT inherits the permissions of its creator. Use a PAT created by:

  <!-- -->

  * A Tableau server or site administrator
  * A data source owner or a project leader

## Orchestrate downstream exposures[​](#orchestrate-downstream-exposures "Direct link to Orchestrate downstream exposures")

To orchestrate downstream exposures and see the refresh happen automatically during scheduled dbt jobs:

1. In the dbt, click **Deploy**, then **Environments**, and select the **Environment variables** tab.

2. Click **Add variable** and set the [environment level variable](https://docs.getdbt.com/docs/build/environment-variables.md#setting-and-overriding-environment-variables) `DBT_ACTIVE_EXPOSURES` to `1` within the environment you want the refresh to happen.

3. Then set the `DBT_ACTIVE_EXPOSURES_BUILD_AFTER` to control the maximum refresh frequency (in minutes) you want between each exposure refresh.

4. Set the variable to **1440** minutes (24 hours) by default. This means that downstream exposures won’t refresh Tableau extracts more often than this set interval, even if the related models run more frequently.
   <!-- -->
   [![Set the environment variable \`DBT\_ACTIVE\_EXPOSURES\` to \`1\`.](/img/docs/cloud-integrations/auto-exposures/active-exposures-env-var.jpg?v=2 "Set the environment variable `DBT_ACTIVE_EXPOSURES` to `1`.")](#)Set the environment variable \`DBT\_ACTIVE\_EXPOSURES\` to \`1\`.

5. Run a job in production. You will see the update each time a job runs in production.
   <!-- -->
   * If a job runs before the set interval has passed, dbt skips the downstream exposure refresh and marks it as `skipped` in the job logs.

6. View the downstream exposure logs in the dbt run job logs.

   <!-- -->

   [![View the downstream exposure logs in the dbt run job logs.](/img/docs/cloud-integrations/auto-exposures/active-exposure-log.jpg?v=2 "View the downstream exposure logs in the dbt run job logs.")](#)View the downstream exposure logs in the dbt run job logs.

   * View more details in the debug logs for any troubleshooting.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
