# Deploy dbt

Use dbt's capabilities to seamlessly run a dbt job in production or staging environments. Rather than run dbt commands manually from the command line, you can leverage the [dbt's in-app scheduling](https://docs.getdbt.com/docs/deploy/job-scheduler.md) to automate how and when you execute dbt.

The dbt platform offers the easiest and most reliable way to run your dbt project in production. Effortlessly promote high quality code from development to production and build fresh data assets that your business intelligence tools and end users query to make business decisions. Deploying with dbt lets you:

* Keep production data fresh on a timely basis
* Ensure CI and production pipelines are efficient
* Identify the root cause of failures in deployment environments
* Maintain high-quality code and data in production
* Gain visibility into the [health](https://docs.getdbt.com/docs/explore/data-tile.md) of deployment jobs, models, and tests
* Uses [exports](https://docs.getdbt.com/docs/use-dbt-semantic-layer/exports.md) to write [saved queries](https://docs.getdbt.com/docs/build/saved-queries.md) in your data platform for reliable and fast metric reporting
* [Visualize](https://docs.getdbt.com/docs/cloud-integrations/downstream-exposures-tableau.md) and [orchestrate](https://docs.getdbt.com/docs/cloud-integrations/orchestrate-exposures.md) downstream exposures to understand how models are used in downstream tools and proactively refresh the underlying data sources during scheduled dbt jobs. [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")
* Use [dbt's Git repository caching](https://docs.getdbt.com/docs/cloud/account-settings.md#git-repository-caching) to protect against third-party outages and improve job run reliability. [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")
* Use [Hybrid projects](https://docs.getdbt.com/docs/deploy/hybrid-projects.md) to upload dbt artifacts into the dbt platform for central visibility, cross-project referencing, and easier collaboration. [Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing") Preview

Before continuing, make sure you understand dbt's approach to [deployment environments](https://docs.getdbt.com/docs/deploy/deploy-environments.md).

Learn how to use dbt's features to help your team ship timely and quality production data more easily.

## Deploy with dbt[​](#deploy-with-dbt "Direct link to Deploy with dbt")

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/deploy/job-scheduler.md)

#### [Job scheduler](https://docs.getdbt.com/docs/deploy/job-scheduler.md)

[The job scheduler is the backbone of running jobs in the dbt platform, bringing power and simplicity to building data pipelines in both continuous integration and production environments.](https://docs.getdbt.com/docs/deploy/job-scheduler.md)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/deploy/deploy-jobs.md)

#### [Deploy jobs](https://docs.getdbt.com/docs/deploy/deploy-jobs.md)

[Create and schedule jobs for the job scheduler to run.](https://docs.getdbt.com/docs/deploy/deploy-jobs.md)

<br />

<br />

[Runs on a schedule, by API, or after another job completes.](https://docs.getdbt.com/docs/deploy/deploy-jobs.md)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/deploy/state-aware-about.md)

#### [State-aware orchestration](https://docs.getdbt.com/docs/deploy/state-aware-about.md)

[Intelligently determines which models to build by detecting changes in code or data at each job run.](https://docs.getdbt.com/docs/deploy/state-aware-about.md)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/deploy/continuous-integration.md)

#### [Continuous integration](https://docs.getdbt.com/docs/deploy/continuous-integration.md)

[Set up CI checks so you can build and test any modified code in a staging environment when you open PRs and push new commits to your dbt repository.](https://docs.getdbt.com/docs/deploy/continuous-integration.md)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/deploy/continuous-deployment.md)

#### [Continuous deployment](https://docs.getdbt.com/docs/deploy/continuous-deployment.md)

[Set up merge jobs to ensure the latest code changes are always in production when pull requests are merged to your Git repository.](https://docs.getdbt.com/docs/deploy/continuous-deployment.md)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/deploy/job-commands.md)

#### [Job commands](https://docs.getdbt.com/docs/deploy/job-commands.md)

[Configure which dbt commands to execute when running a dbt job.](https://docs.getdbt.com/docs/deploy/job-commands.md)

<br />

## Monitor jobs and alerts[​](#monitor-jobs-and-alerts "Direct link to Monitor jobs and alerts")

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/deploy/orchestrate-exposures.md)

#### [Visualize and orchestrate exposures](https://docs.getdbt.com/docs/deploy/orchestrate-exposures.md)

[Learn how to use dbt to automatically generate downstream exposures from dashboards and proactively refresh the underlying data sources during scheduled dbt jobs.](https://docs.getdbt.com/docs/deploy/orchestrate-exposures.md)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/deploy/artifacts.md)

#### [Artifacts](https://docs.getdbt.com/docs/deploy/artifacts.md)

[dbt generates and saves artifacts for your project, which it uses to power features like creating docs for your project and reporting the freshness of your sources.](https://docs.getdbt.com/docs/deploy/artifacts.md)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/deploy/job-notifications.md)

#### [Job notifications](https://docs.getdbt.com/docs/deploy/job-notifications.md)

[Receive email or Slack channel notifications when a job run succeeds, fails, or is canceled so you can respond quickly and begin remediation if necessary.](https://docs.getdbt.com/docs/deploy/job-notifications.md)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/deploy/model-notifications.md)

#### [Model notifications](https://docs.getdbt.com/docs/deploy/model-notifications.md)

[Receive email notifications in real time about issues encountered by your models and tests while a job is running.](https://docs.getdbt.com/docs/deploy/model-notifications.md)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/deploy/run-visibility.md)

#### [Run visibility](https://docs.getdbt.com/docs/deploy/run-visibility.md)

[View the history of your runs and the model timing dashboard to help identify where improvements can be made to the scheduled jobs.](https://docs.getdbt.com/docs/deploy/run-visibility.md)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/deploy/retry-jobs.md)

#### [Retry jobs](https://docs.getdbt.com/docs/deploy/retry-jobs.md)

[Rerun your errored jobs from start or the failure point.](https://docs.getdbt.com/docs/deploy/retry-jobs.md)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/deploy/source-freshness.md)

#### [Source freshness](https://docs.getdbt.com/docs/deploy/source-freshness.md)

[Enable snapshots to capture the freshness of your data sources and configure how frequent these snapshots should be taken. This can help you determine whether your source data freshness is meeting your SLAs.](https://docs.getdbt.com/docs/deploy/source-freshness.md)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/deploy/webhooks.md)

#### [Webhooks](https://docs.getdbt.com/docs/deploy/webhooks.md)

[Create outbound webhooks to send events about your dbt jobs' statuses to other systems in your organization.](https://docs.getdbt.com/docs/deploy/webhooks.md)

<br />

## Hybrid projects [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing") Preview[​](#hybrid-projects-- "Direct link to hybrid-projects--")

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/docs/deploy/hybrid-projects.md)

#### [Hybrid projects](https://docs.getdbt.com/docs/deploy/hybrid-projects.md)

[Use Hybrid projects to upload dbt Core artifacts into the dbt platform for central visibility, cross-project referencing, and easier collaboration.](https://docs.getdbt.com/docs/deploy/hybrid-projects.md)

<br />

## Related docs[​](#related-docs "Direct link to Related docs")

* [Use exports to materialize saved queries](https://docs.getdbt.com/docs/use-dbt-semantic-layer/exports.md)
* [Integrate with other orchestration tools](https://docs.getdbt.com/docs/deploy/deployment-tools.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
