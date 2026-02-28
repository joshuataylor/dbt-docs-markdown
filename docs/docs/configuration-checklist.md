# dbt platform configuration checklist [Starter](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

So, you've created a new cloud-hosted dbt platform account, and you're ready to explore its lightning-fast and intuitive features. Welcome! Before you begin, let’s ensure your account is properly configured so that you can easily onboard new users and take advantage of all the integrations dbt has to offer.

For most organizations, this will require some collaboration with IT and/or security teams. Depending on the features you're using, you may need some of the following admin personas to help you get set up:

* Data warehouse (Snowflake, BigQuery, Databricks, etc.)
* Access control (Okta, Entra ID, Google, SAML 2.0)
* Git (GitHub, GitLab, Azure DevOps, etc.)

This checklist ensures you have everything in the right place, allowing you to deploy quickly and without any bottlenecks.

## Data warehouse[​](#data-warehouse "Direct link to Data warehouse")

The dbt platform supports [global connections](https://docs.getdbt.com/docs/cloud/connect-data-platform/about-connections.md#connection-management) for your data warehouses. This means that a single configured connection can be used across multiple projects and environments. The dbt platform supports multiple data warehouse connections, including (but not limited to) BigQuery, Databricks, Redshift, and Snowflake. One of the earliest account configuration steps you'll want to take is ensuring you have a working connection:

* <!-- -->
  Use the [connection set up documentation](https://docs.getdbt.com/docs/cloud/connect-data-platform/about-connections.md) to configure the data warehouse connection of your choice.
* <!-- -->
  Verify that dbt developers have proper roles and access in your data warehouse(s).
* <!-- -->
  Be sure the data warehouse has real data you can reference. This can be production or development data. We have a sandbox e-commerce project called [The Jaffle Shop](https://github.com/dbt-labs/jaffle-shop) that you can use if you prefer. The Jaffle Shop includes mock data and ready-to-run models!
* <!-- -->
  Whether starting a brand new project or importing an existing dbt Core project, you'll want to make sure you have the [proper structure configured](https://docs.getdbt.com/docs/build/projects.md).
  <!-- -->
  * <!-- -->
    If you are migrating from Core, there are some important things you'll need to know, so check out our [migration guide](https://docs.getdbt.com/guides/core-cloud-2.md?step=1).
* <!-- -->
  Your users will need to [configure their credentials](https://docs.getdbt.com/docs/cloud/studio-ide/develop-in-studio.md#get-started-with-the-cloud-ide) to connect to the development environment in the dbt Studio IDE.
  <!-- -->
  * <!-- -->
    Ensure that all users who need access to work in the IDE have a [developer license](https://docs.getdbt.com/docs/cloud/manage-access/seats-and-users.md) assigned in your account.
* <!-- -->
  dbt models are primarily written as [SELECT statements](https://docs.getdbt.com/docs/build/sql-models.md), so an early step for measuring success is having a developer run a simple select statement in the IDE and validating the results.
  <!-- -->
  * <!-- -->
    You can also verify the connection by running basic SQL queries using [dbt Insights](https://docs.getdbt.com/docs/explore/access-dbt-insights.md).
* <!-- -->
  Create a single model and ensure that you can [run it](https://docs.getdbt.com/reference/dbt-commands.md) successfully.
  <!-- -->
  * <!-- -->
    For an easy to use drag-and-drop interface, try creating it with [dbt Canvas](https://docs.getdbt.com/docs/cloud/canvas.md).
* <!-- -->
  Create a service account with proper access for your [production jobs](https://docs.getdbt.com/docs/deploy/jobs.md).

## Git configuration[​](#git-configuration "Direct link to Git configuration")

Git is, for many dbt environments, the backbone of your project. Git repositories are where your dbt files will live and where your developers will collaborate and manage version control of your project.

* <!-- -->

  Configure a [Git repository](https://docs.getdbt.com/docs/cloud/git/git-configuration-in-dbt-cloud.md) for your account. dbt supports integrations with:

  <!-- -->

  * [GitHub](https://docs.getdbt.com/docs/cloud/git/connect-github.md)
  * [GitLab](https://docs.getdbt.com/docs/cloud/git/connect-gitlab.md)
  * [Azure DevOps](https://docs.getdbt.com/docs/cloud/git/connect-azure-devops.md)
  * Other providers using [Git clone](https://docs.getdbt.com/docs/cloud/git/import-a-project-by-git-url.md)
  * If you aren't ready to integrate with an existing Git solution, dbt can provide you with a [managed Git repository](https://docs.getdbt.com/docs/cloud/git/managed-repository.md).

* <!-- -->
  Ensure developers can [checkout](https://docs.getdbt.com/docs/cloud/git/version-control-basics.md#git-overview) a new branch in your repo.

* <!-- -->
  Ensure developers in the IDE can [commit changes](https://docs.getdbt.com/docs/cloud/studio-ide/ide-user-interface.md#basic-layout).

## Environments and jobs[​](#environments-and-jobs "Direct link to Environments and jobs")

[Environments](https://docs.getdbt.com/docs/environments-in-dbt.md) separate your development data from your production data. dbt supports two environment types: Development and Deployment. There are three types of deployment environments:

* Production - One per project
* Staging - One per project
* General - Multiple per project

Additionally, you will have only one `Development` environment per project, but each developer will have their own unique access to the IDE, separate from the work of other developers.

[Jobs](https://docs.getdbt.com/docs/deploy/jobs.md) dictate which commands are run in your environments and can be triggered manually, on a schedule, by other jobs, by APIs, or when pull requests are committed or merged.

Once you connect your data warehouse and complete the Git integration, you can configure environments and jobs:

* <!-- -->
  Start by creating a new [Development environment](https://docs.getdbt.com/docs/dbt-cloud-environments.md#create-a-development-environment) for your project.
* <!-- -->
  Create a [Production Deployment environment](https://docs.getdbt.com/docs/deploy/deploy-environments.md).
  <!-- -->
  * <!-- -->
    (Optional) Create an additional Staging or General environment.
* [ ] [Create and schedule](https://docs.getdbt.com/docs/deploy/deploy-jobs.md#create-and-schedule-jobs) a deployment job.
  <!-- -->
  * <!-- -->
    Validate the job by manually running it first.
* <!-- -->
  If needed, configure different databases for your environments.

## User access[​](#user-access "Direct link to User access")

The dbt platform offers a variety of access control tools that you can leverage to grant or revoke user access, configure RBAC, and assign user licenses and permissions.

* <!-- -->
  Manually [invite users](https://docs.getdbt.com/docs/cloud/manage-access/invite-users.md) to the dbt platform, and they can authenticate using [MFA (SMS or authenticator app)](https://docs.getdbt.com/docs/cloud/manage-access/mfa.md).
* <!-- -->
  Configure [single sign-on or OAuth](https://docs.getdbt.com/docs/cloud/manage-access/sso-overview.md) for advanced access control. [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing") accounts only.
  <!-- -->
  * <!-- -->
    Create [SSO mappings](https://docs.getdbt.com/docs/cloud/manage-access/about-user-access.md#sso-mappings-) for groups
* Configure [System for Cross-Domain Identity Management (SCIM)](https://docs.getdbt.com/docs/cloud/manage-access/scim.md) if available for your IdP.
* <!-- -->
  Ensure invited users are able to connect to the data warehouse from their personal profile.
* [ ] [Create groups](https://docs.getdbt.com/docs/cloud/manage-access/about-user-access.md#create-new-groups-) with granular permission sets assigned.
* <!-- -->
  Create [RBAC rules](https://docs.getdbt.com/docs/cloud/manage-access/about-user-access#role-based-access-control-) to assign users to groups and permission sets upon sign in. [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing") accounts only.
* <!-- -->
  Enforce SSO for all non-admin users, and MFA is enforced for all password-based logins.

## Continue the journey[​](#continue-the-journey "Direct link to Continue the journey")

Once you've completed this checklist, you're ready to start your dbt platform journey, but that journey has only just begun. Explore these additional resources to support you along the way:

* <!-- -->
  Review the [guides](https://docs.getdbt.com/guides.md) for quickstarts to help you get started with projects and features.
* <!-- -->
  Take a [dbt Learn](https://learn.getdbt.com/catalog) hands-on course.
* <!-- -->
  Review our [best practices](https://docs.getdbt.com/best-practices.md) for practical advice on structuring and deploying your dbt projects.
* <!-- -->
  Become familiar with the [references](https://docs.getdbt.com/reference/references-overview.md), as they are the product dictionary and offer detailed implementation examples.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
