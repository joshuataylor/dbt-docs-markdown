# Deployment environments

dbt platformⓘ

Deployment environments in dbt are crucial for deploying dbt jobs in production and using features or integrations that depend on dbt metadata or results. To execute dbt, environments determine the settings used during job runs, including:

* The version of dbt Core that will be used to run your project
* The warehouse connection information (including the target database/schema settings)
* The version of your code to execute

A dbt project can have multiple deployment environments, providing you the flexibility and customization to tailor the execution of dbt jobs. You can use deployment environments to [create and schedule jobs](https://docs.getdbt.com/docs/deploy/deploy-jobs.md#create-and-schedule-jobs), [enable continuous integration](https://docs.getdbt.com/docs/deploy/continuous-integration.md), or more based on your specific needs or requirements.

Learn how to manage dbt environments

To learn different approaches to managing dbt environments and recommendations for your organization's unique needs, read [dbt environment best practices](https://docs.getdbt.com/guides/set-up-ci.md).

Learn more about development vs. deployment environments in [dbt Environments](https://docs.getdbt.com/docs/dbt-platform-environments.md).

There are three types of deployment environments:

* **Production**: Environment for transforming data and building pipelines for production use.
* **Staging**: Environment for working with production tools while limiting access to production data.
* **General**: General use environment for deployment development.

We highly recommend using the `Production` environment type for the final, source of truth deployment data. There can be only one environment marked for final production workflows and we don't recommend using a `General` environment for this purpose.

## Create a deployment environment[​](#create-a-deployment-environment "Direct link to Create a deployment environment")

To create a new dbt deployment environment, navigate to **Orchestration** > **Environments** and then click **Create Environment**. Select **Deployment** as the environment type. The option will be greyed out if you already have a development environment.

[![Navigate to Orchestration > Environments to create a deployment environment](/img/docs/dbt-platform/platform-configuring-dbt-platform/create-deploy-env.png?v=2 "Navigate to Orchestration > Environments to create a deployment environment")](#)Navigate to Orchestration > Environments to create a deployment environment

### Set as production environment[​](#set-as-production-environment "Direct link to Set as production environment")

In dbt, each project can have one designated deployment environment, which serves as its production environment. This production environment is *essential* for using features like Catalog and cross-project references. It acts as the source of truth for the project's production state in dbt.

[![Set your production environment as the default environment in your Environment Settings](/img/docs/dbt-platform/using-dbt-platform/prod-settings-1.png?v=2 "Set your production environment as the default environment in your Environment Settings")](#)Set your production environment as the default environment in your Environment Settings

### Semantic Layer[​](#semantic-layer "Direct link to Semantic Layer")

For customers using the Semantic Layer, the next section of environment settings is the Semantic Layer configurations. [The Semantic Layer setup guide](https://docs.getdbt.com/docs/use-dbt-semantic-layer/setup-sl.md) has the most up-to-date setup instructions.

You can also leverage the dbt Job scheduler to [validate your semantic nodes in a CI job](https://docs.getdbt.com/docs/deploy/ci-jobs.md#semantic-validations-in-ci) to ensure code changes made to dbt models don't break these metrics.

## Staging environment[​](#staging-environment "Direct link to Staging environment")

Use a staging environment to grant developers access to deployment workflows and tools while controlling access to production data. Staging environments enable you to achieve more granular control over permissions, data warehouse connections, and data isolation — within the purview of a single project in dbt.

### Git workflow[​](#git-workflow "Direct link to Git workflow")

You can approach this in a couple of ways, but the most straightforward is configuring staging with a long-living branch (for example, `staging`) similar to, but separate from the primary branch (for example, `main`).

In this scenario, the workflows would ideally move upstream from the Development environment -> Staging environment -> Production environment with developer branches feeding into the `staging` branch, then ultimately merging into `main`. In many cases, the `main` and `staging` branches will be identical after a merge and remain until the next batch of changes from the `development` branches are ready to be elevated. We recommend setting branch protection rules on `staging` similar to `main`.

Some customers prefer to connect Development and Staging to their `main` branch and then cut release branches on a regular cadence (daily or weekly), which feeds into Production.

### Why use a staging environment[​](#why-use-a-staging-environment "Direct link to Why use a staging environment")

These are the primary motivations for using a staging environment:

1. An additional validation layer before changes are deployed into production. You can deploy, test, and explore your dbt models in staging.
2. Clear isolation between development workflows and production data. It enables developers to work in metadata-powered ways, using features like deferral and cross-project references, without accessing data in production deployments.
3. Provide developers with the ability to create, edit, and trigger ad hoc jobs in the staging environment, while keeping the production environment locked down using [environment-level permissions](https://docs.getdbt.com/docs/platform/manage-access/environment-permissions.md).

**Conditional configuration of sources** enables you to point to "prod" or "non-prod" source data, depending on the environment you're running in. For example, this source will point to `<DATABASE>.sensitive_source.table_with_pii`, where `<DATABASE>` is dynamically resolved based on an environment variable.

models/sources.yml

```yaml
sources:
  - name: sensitive_source
    database: "{{ env_var('SENSITIVE_SOURCE_DATABASE') }}"
    tables:
      - name: table_with_pii
```

There is exactly one source (`sensitive_source`), and all downstream dbt models select from it as `{{ source('sensitive_source', 'table_with_pii') }}`. The code in your project and the shape of the DAG remain consistent across environments. By setting it up in this way, rather than duplicating sources, you get some important benefits.

**Cross-project references in dbt Mesh:** Let's say you have `Project B` downstream of `Project A` with cross-project refs configured in the models. When developers work in the IDE for `Project B`, cross-project refs will resolve to the staging environment of `Project A`, rather than production. You'll get the same results with those refs when jobs are run in the staging environment. Only the production environment will reference the production data, keeping the data and access isolated without needing separate projects.

**Faster development enabled by deferral:** If `Project B` also has a staging deployment, then references to unbuilt upstream models<!-- --> and [user-defined functions (UDFs)](https://docs.getdbt.com/docs/build/udfs.md) within `Project B` will resolve to that environment using [deferral](https://docs.getdbt.com/docs/platform/about-defer.md), rather than resolving to the models<!-- --> and functions<!-- --> in production. This saves developers time and warehouse spend, while preserving clear separation of environments.

Finally, the staging environment has its own view in [Catalog](https://docs.getdbt.com/docs/explore/explore-projects.md), giving you a full view of your prod and pre-prod data.

[![Explore in a staging environment](/img/docs/collaborate/dbt-explorer/explore-staging-env.png?v=2 "Explore in a staging environment")](#)Explore in a staging environment

### Create a Staging environment[​](#create-a-staging-environment "Direct link to Create a Staging environment")

[![Create a staging environment](/img/docs/dbt-platform/platform-configuring-dbt-platform/create-staging-environment.png?v=2 "Create a staging environment")](#)Create a staging environment

Follow the steps outlined in [connection profiles](https://docs.getdbt.com/docs/platform/about-profiles.md) to complete the remainder of the environment setup.

We recommend that the data warehouse credentials be for a dedicated user or service principal.

## Deployment connection[​](#deployment-connection "Direct link to Deployment connection")

Warehouse Connections

Warehouse connections are created and managed at the account-level for dbt accounts and assigned to an environment. To change warehouse type, we recommend creating a new environment.

Each project can have multiple connections (Snowflake account, Redshift host, Bigquery project, Databricks host, and so on.) of the same warehouse type. Some details of that connection (databases/schemas/and so on.) can be overridden within this section of the dbt environment settings.

This section determines the exact location in your warehouse dbt should target when building warehouse objects! This section will look a bit different depending on your warehouse provider.

For all warehouses, use [extended attributes](https://docs.getdbt.com/docs/dbt-platform-environments.md#extended-attributes) to override missing or inactive (grayed-out) settings.

* Postgres
* Redshift
* Snowflake
* Bigquery
* Spark
* Databricks

This section will not appear if you are using Postgres, as all values are inferred from the project's connection. Use [extended attributes](https://docs.getdbt.com/docs/dbt-platform-environments.md#extended-attributes) to override these values.

This section will not appear if you are using Redshift, as all values are inferred from the project's connection. Use [extended attributes](https://docs.getdbt.com/docs/dbt-platform-environments.md#extended-attributes) to override these values.

[![Snowflake Deployment Connection Settings](/img/docs/collaborate/snowflake-deploy-env-deploy-connection.png?v=2 "Snowflake Deployment Connection Settings")](#)Snowflake Deployment Connection Settings

#### Editable fields[​](#editable-fields "Direct link to Editable fields")

* **Role**: Snowflake role
* **Database**: Target database
* **Warehouse**: Snowflake warehouse

This section will not appear if you are using Bigquery, as all values are inferred from the project's connection. Use [extended attributes](https://docs.getdbt.com/docs/dbt-platform-environments.md#extended-attributes) to override these values.

This section will not appear if you are using Spark, as all values are inferred from the project's connection. Use [extended attributes](https://docs.getdbt.com/docs/dbt-platform-environments.md#extended-attributes) to override these values.

[![Databricks Deployment Connection Settings](/img/docs/collaborate/databricks-deploy-env-deploy-connection.png?v=2 "Databricks Deployment Connection Settings")](#)Databricks Deployment Connection Settings

#### Editable fields[​](#editable-fields-1 "Direct link to Editable fields")

* **Catalog** (optional): [Unity Catalog namespace](https://docs.getdbt.com/docs/local/connect-data-platform/databricks-setup.md)

### Connection profiles[​](#connection-profiles "Direct link to Connection profiles")

Deployment credentials are managed through connection profiles, which are created at the project level and assigned to deployment environments. Profiles define the credentials and attributes dbt uses to connect to your warehouse.

To configure credentials for this environment, refer to [About dbt platform profiles](https://docs.getdbt.com/docs/platform/about-profiles.md).

## Delete an environment[​](#delete-an-environment "Direct link to Delete an environment")

<!-- -->

Deleting an environment automatically deletes its associated job(s). If you want to keep those jobs, move them to a different environment first.

Follow these steps to delete an environment in dbt:

1. Navigate to **Orchestration** > **Environments**.
2. Select the environment you want to delete.
3. Click **Settings** on the top right of the page and then click **Edit**.
4. Scroll to the bottom of the page and click **Delete** to delete the environment.

[![Delete an environment](/img/docs/dbt-platform/platform-configuring-dbt-platform/delete-environment.png?v=2 "Delete an environment")](#)Delete an environment

5. Confirm your action in the pop-up by clicking **Confirm delete** in the bottom right to delete the environment immediately. This action cannot be undone. However, you can create a new environment with the same information if the deletion was made in error.
6. Refresh your page and the deleted environment should now be gone. To delete multiple environments, you'll need to perform these steps to delete each one.

If you're having any issues, feel free to [contact us](mailto:support@getdbt.com) for additional help.

## Related docs[​](#related-docs "Direct link to Related docs")

* [dbt environment best practices](https://docs.getdbt.com/guides/set-up-ci.md)
* [Deploy jobs](https://docs.getdbt.com/docs/deploy/deploy-jobs.md)
* [CI jobs](https://docs.getdbt.com/docs/deploy/continuous-integration.md)
* [Delete a job or environment in dbt](https://docs.getdbt.com/faqs/Environments/delete-environment-job.md)
