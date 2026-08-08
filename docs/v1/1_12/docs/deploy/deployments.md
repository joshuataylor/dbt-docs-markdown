# Deploy dbt

dbt platformⓘ

Use the dbt platform's capabilities to seamlessly run a dbt job in production or staging environments. Rather than run dbt commands manually from the command line, you can leverage the [dbt platform's in-app scheduling](./job-scheduler.md) to automate how and when you execute dbt.

The dbt platform offers the easiest and most reliable way to run your dbt project in production. Effortlessly promote high quality code from development to production and build fresh data assets that your business intelligence tools and end users query to make business decisions. Deploying with dbt lets you:

* Keep production data fresh on a timely basis
* Ensure CI and production pipelines are efficient
* Identify the root cause of failures in deployment environments
* Maintain high-quality code and data in production
* Gain visibility into the [health](../explore/data-tile.md) of deployment jobs, models, and tests
* Uses [exports](../use-dbt-semantic-layer/exports.md) to write [saved queries](../build/saved-queries.md) in your data platform for reliable and fast metric reporting
* [Visualize](../platform-integrations/downstream-exposures-tableau.md) and [orchestrate](../platform-integrations/orchestrate-exposures.md) downstream exposures to understand how models are used in downstream tools and proactively refresh the underlying data sources during scheduled dbt jobs. [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")
* Use [dbt's Git repository caching](../platform/account-settings.md#git-repository-caching) to protect against third-party outages and improve job run reliability. [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")
* Use [Hybrid projects](./hybrid-projects.md) to upload dbt artifacts into the dbt platform for central visibility, cross-project referencing, and easier collaboration. [Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing") Preview

Before continuing, make sure you understand dbt's approach to [deployment environments](./deploy-environments.md).

Learn how to use dbt's features to help your team ship timely and quality production data more easily.

## Deploy with dbt[​](#deploy-with-dbt "Direct link to Deploy with dbt")

[![](/img/icons/dbt-bit.svg)](./job-scheduler.md)

#### [Job scheduler](./job-scheduler.md)

[The job scheduler is the backbone of running jobs in the dbt platform, bringing power and simplicity to building data pipelines in both continuous integration and production environments.](./job-scheduler.md)

[![](/img/icons/dbt-bit.svg)](./deploy-jobs.md)

#### [Deploy jobs](./deploy-jobs.md)

[Create and schedule jobs for the job scheduler to run.](./deploy-jobs.md)

<br />

<br />

[Runs on a schedule, by API, or after another job completes.](./deploy-jobs.md)

[![](/img/icons/dbt-bit.svg)](./dbt-state-about.md)

#### [dbt State](./dbt-state-about.md)

[Intelligently determines which models to build by detecting changes in code or data at each job run.](./dbt-state-about.md)

[![](/img/icons/dbt-bit.svg)](./continuous-integration.md)

#### [Continuous integration](./continuous-integration.md)

[Set up CI checks so you can build and test any modified code in a staging environment when you open PRs and push new commits to your dbt repository.](./continuous-integration.md)

[![](/img/icons/dbt-bit.svg)](./continuous-deployment.md)

#### [Continuous deployment](./continuous-deployment.md)

[Set up merge jobs to ensure the latest code changes are always in production when pull requests are merged to your Git repository.](./continuous-deployment.md)

[![](/img/icons/dbt-bit.svg)](./job-commands.md)

#### [Job commands](./job-commands.md)

[Configure which dbt commands to execute when running a dbt job.](./job-commands.md)

<br />

## Monitor jobs and alerts[​](#monitor-jobs-and-alerts "Direct link to Monitor jobs and alerts")

[![](/img/icons/dbt-bit.svg)](./orchestrate-exposures.md)

#### [Visualize and orchestrate exposures](./orchestrate-exposures.md)

[Learn how to use dbt to automatically generate downstream exposures from dashboards and proactively refresh the underlying data sources during scheduled dbt jobs.](./orchestrate-exposures.md)

[![](/img/icons/dbt-bit.svg)](./artifacts.md)

#### [Artifacts](./artifacts.md)

[dbt generates and saves artifacts for your project, which it uses to power features like creating docs for your project and reporting the freshness of your sources.](./artifacts.md)

[![](/img/icons/dbt-bit.svg)](./job-notifications.md)

#### [Job notifications](./job-notifications.md)

[Receive email or Slack channel notifications when a job run succeeds, fails, or is canceled so you can respond quickly and begin remediation if necessary.](./job-notifications.md)

[![](/img/icons/dbt-bit.svg)](./model-notifications.md)

#### [Model notifications](./model-notifications.md)

[Receive email notifications in real time about issues encountered by your models and tests while a job is running.](./model-notifications.md)

[![](/img/icons/dbt-bit.svg)](./run-visibility.md)

#### [Run visibility](./run-visibility.md)

[View the history of your runs and the model timing dashboard to help identify where improvements can be made to the scheduled jobs.](./run-visibility.md)

[![](/img/icons/dbt-bit.svg)](./retry-jobs.md)

#### [Retry jobs](./retry-jobs.md)

[Rerun your errored jobs from start or the failure point.](./retry-jobs.md)

[![](/img/icons/dbt-bit.svg)](./source-freshness.md)

#### [Source freshness](./source-freshness.md)

[Enable snapshots to capture the freshness of your data sources and configure how frequent these snapshots should be taken. This can help you determine whether your source data freshness is meeting your SLAs.](./source-freshness.md)

[![](/img/icons/dbt-bit.svg)](./webhooks.md)

#### [Webhooks](./webhooks.md)

[Create outbound webhooks to send events about your dbt jobs' statuses to other systems in your organization.](./webhooks.md)

<br />

## Hybrid projects [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing") Preview[​](#hybrid-projects-- "Direct link to hybrid-projects--")

[![](/img/icons/dbt-bit.svg)](./hybrid-projects.md)

#### [Hybrid projects](./hybrid-projects.md)

[Use Hybrid projects to upload dbt Core artifacts into the dbt platform for central visibility, cross-project referencing, and easier collaboration.](./hybrid-projects.md)

<br />

## Related docs[​](#related-docs "Direct link to Related docs")

* [Use exports to materialize saved queries](../use-dbt-semantic-layer/exports.md)
* [Integrate with other orchestration tools](./deployment-tools.md)
