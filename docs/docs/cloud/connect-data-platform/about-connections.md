# About data platform connections

The dbt platform can connect with a variety of data platform providers. Expand the sections below to know the supported data platforms for dbt Core and the dbt Fusion engine:

| Connection                                                                                                                                                                                                                            | Available on Latest | Available on Fusion[Private preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles") |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [AlloyDB](https://docs.getdbt.com/docs/cloud/connect-data-platform/connect-postgresql-alloydb.md)                                                                                                                                     | ✅                  | ❌                                                                                                                                                                      |
| [Amazon Athena](https://docs.getdbt.com/docs/cloud/connect-data-platform/connect-amazon-athena.md)                                                                                                                                    | ✅                  | ❌                                                                                                                                                                      |
| [Amazon Redshift](https://docs.getdbt.com/docs/cloud/connect-data-platform/connect-redshift.md)                                                                                                                                       | ✅                  | ✅                                                                                                                                                                      |
| [Apache Spark](https://docs.getdbt.com/docs/cloud/connect-data-platform/connect-apache-spark.md)                                                                                                                                      | ✅                  | ❌                                                                                                                                                                      |
| [Azure Synapse Analytics](https://docs.getdbt.com/docs/cloud/connect-data-platform/connect-azure-synapse-analytics.md)                                                                                                                | ✅                  | ❌                                                                                                                                                                      |
| [Databricks](https://docs.getdbt.com/docs/cloud/connect-data-platform/connect-databricks.md)                                                                                                                                          | ✅                  | ✅                                                                                                                                                                      |
| [Google BigQuery](https://docs.getdbt.com/docs/cloud/connect-data-platform/connect-bigquery.md)                                                                                                                                       | ✅                  | ✅                                                                                                                                                                      |
| [Microsoft Fabric](https://docs.getdbt.com/docs/cloud/connect-data-platform/connect-microsoft-fabric.md)                                                                                                                              | ✅                  | ❌                                                                                                                                                                      |
| [PostgreSQL](https://docs.getdbt.com/docs/cloud/connect-data-platform/connect-postgresql-alloydb.md)                                                                                                                                  | ✅                  | ❌                                                                                                                                                                      |
| [Snowflake](https://docs.getdbt.com/docs/cloud/connect-data-platform/connect-snowflake.md)                                                                                                                                            | ✅                  | ✅                                                                                                                                                                      |
| [Starburst or Trino](https://docs.getdbt.com/docs/cloud/connect-data-platform/connect-starburst-trino.md)                                                                                                                             | ✅                  | ❌                                                                                                                                                                      |
| [Teradata](https://docs.getdbt.com/docs/cloud/connect-data-platform/connect-teradata.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles") | ✅                  | ❌                                                                                                                                                                      |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

To connect to your database in dbt:

1. Click your account name at the bottom of the left-side menu and click **Account settings**.
2. Select **Connections** from the top left, and from there, click **New connection**.

[![Choose a connection](/img/docs/connect-data-platform/choose-a-connection.png?v=2 "Choose a connection")](#)Choose a connection

These connection instructions provide the basic fields required for configuring a data platform connection in dbt. For more detailed guides, which include demo project data, read our [Quickstart guides](https://docs.getdbt.com/guides.md).

### Supported authentication methods[​](#supported-authentication-methods "Direct link to Supported authentication methods")

The following tables show which authentication types are supported for each connection available on the dbt platform:

* dbt Core
* dbt Fusion

| Integration | User credentials | Service account credentials | Warehouse OAuth for users | External OAuth for users | Service-to-service OAuth | SSH | Private connectivity support\*\* |
| ----------- | ---------------- | --------------------------- | ------------------------- | ------------------------ | ------------------------ | --- | -------------------------------- |
| Snowflake   | ✅               | ✅                          | ✅                        | ✅                       | ❌                       | ❌  | ✅                               |
| BigQuery    | ✅               | ✅                          | ✅                        | ❌                       | ✅                       | ❌  | ✅                               |
| Databricks  | ✅               | ✅                          | ✅                        | ❌                       | ❌                       | ❌  | ✅                               |
| Redshift    | ✅               | ❌                          | ❌                        | ✅                       | ❌                       | ✅  | ✅                               |
| Fabric      | ✅               | ✅                          | ❌                        | ❌                       | ❌                       | ❌  | ❌                               |
| Synapse     | ✅               | ✅                          | ❌                        | ❌                       | ❌                       | ❌  | ✅                               |
| Trino       | ✅               | ❌                          | ❌                        | ❌                       | ❌                       | ❌  | ❌                               |
| Teradata    | ✅               | ❌                          | ❌                        | ❌                       | ❌                       | ❌  | ✅                               |
| AWS Athena  | ✅               | ✅                          | ❌                        | ❌                       | ❌                       | ❌  | ✅                               |
| Postgres    | ✅               | ❌                          | ❌                        | ❌                       | ❌                       | ✅  | ✅                               |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

\*\* Private connectivity is only supported for certain cloud providers and deployment types. See [Private connectivity documentation](https://docs.getdbt.com/docs/cloud/secure/private-connectivity/private-connectivity.md) for details.

| Integration | User credentials/token | Service account credentials | Warehouse OAuth for users | External OAuth for users | Service-to-service OAuth | Key/Pair | MFA | SSH | Private connectivity support\*\* |
| ----------- | ---------------------- | --------------------------- | ------------------------- | ------------------------ | ------------------------ | -------- | --- | --- | -------------------------------- |
| Snowflake   | ✅                     | ✅                          | ✅                        | ✅                       | ❌                       | ✅       | ✅  | ❌  | ✅                               |
| BigQuery    | ✅                     | ✅                          | ✅                        | ✅                       | ❌                       | ❌       | ❌  | ❌  | ✅                               |
| Databricks  | ✅                     | ✅                          | ✅                        | ❌                       | ❌                       | ❌       | ❌  | ❌  | ✅                               |
| Redshift    | ✅                     | ❌                          | ❌                        | ❌                       | ❌                       | ❌       | ❌  | ❌  | ✅                               |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

\*\* Private connectivity is only supported for certain cloud providers and deployment types. See [Private connectivity documentation](https://docs.getdbt.com/docs/cloud/secure/private-connectivity/private-connectivity.md) for details.

## Connection management[​](#connection-management "Direct link to Connection management")

Warehouse connections are an account-level resource. You can find them under **Accounts settings** > **Connections**.

Warehouse connections can be re-used across projects. If multiple projects all connect to the same warehouse, you should re-use the same connection to streamline your management operations. Connections are assigned to a project via an [environment](https://docs.getdbt.com/docs/dbt-cloud-environments.md).

[![Connection model](/img/docs/dbt-cloud/cloud-configuring-dbt-cloud/connections-new-model.png?v=2 "Connection model")](#)Connection model

As shown in the image, a project with 2 environments can target between 1 and 2 different connections. If you want to separate your production environment from your non-production environment, assign multiple connections to a single project.

### Migration from project-level connections to account-level connections[​](#migration-from-project-level-connections-to-account-level-connections "Direct link to Migration from project-level connections to account-level connections")

Rolling out account-level connections will not require any interruption of service in your current usage (Studio IDE, CLI, jobs, and so on.).

Why am I prompted to configure a development environment?

If your project did not previously have a development environment, you may be redirected to the project setup page. Your project is still intact. Choose a connection for your new development environment, and you can view all your environments again.

However, to fully utilize the value of account-level connections, you may have to rethink how you assign and use connections across projects and environments.

[![Typical connection setup post rollout](/img/docs/dbt-cloud/cloud-configuring-dbt-cloud/connections-post-rollout.png?v=2 "Typical connection setup post rollout")](#)Typical connection setup post rollout

Please consider the following actions, as the steps you take will depend on the desired outcome.

* The initial clean-up of your connection list

  <!-- -->

  * Delete unused connections with 0 environments.
  * Rename connections with a temporary, descriptive naming scheme to better understand where each is used

[![Post initial clean-up](/img/docs/dbt-cloud/cloud-configuring-dbt-cloud/connections-post-rollout-2.png?v=2 "Post initial clean-up")](#)Post initial clean-up

* Get granular with your connections

  <!-- -->

  * Define an intent for each connection, usually a combination of warehouse/database instance, intended use (dev, prod, etc), and administrative surface (which teams/projects will need to collaborate on the connection)
  * Aim to minimize the need for local overrides (like extended attributes)
  * Come to a consensus on a naming convention. We recommend you name connections after the server hostname and distinct intent/domain/configuration. It will be easier to reuse connections across projects this way

[![Granularity determined](/img/docs/dbt-cloud/cloud-configuring-dbt-cloud/connections-post-rollout-3.png?v=2 "Granularity determined")](#)Granularity determined

* Deduplication (connection list + environment details — not touching extended attributes for now)

  <!-- -->

  * Based of the granularity of your connection details, determine which connections should remain among groups of duplicates, and update every relevant environment to leverage that connection
  * Delete unused connections with 0 environments as you go
  * Deduplicate thoughtfully. If you want connections to be maintained by two different groups of users, you may want to preserve two identical connections to the same warehouse so each can evolve as each group sees fit without impacting the other group
  * Do not update extended attributes at this stage

[![Connections de-duplicated](/img/docs/dbt-cloud/cloud-configuring-dbt-cloud/connections-post-rollout-4.png?v=2 "Connections de-duplicated")](#)Connections de-duplicated

* Normalization

  <!-- -->

  * Understand how new connections should be created to avoid local overrides. If you currently use extended attributes to override the warehouse instance in your production environment - you should instead create a new connection for that instance, and wire your production environment to it, removing the need for the local overrides
  * Create new connections, update relevant environments to target these connections, removing now unecessary local overrides (which may not be all of them!)
  * Test the new wiring by triggering jobs or starting Studio IDE sessions

[![Connections normalized](/img/docs/dbt-cloud/cloud-configuring-dbt-cloud/connections-post-rollout-5.png?v=2 "Connections normalized")](#)Connections normalized

## IP Restrictions[​](#ip-restrictions "Direct link to IP Restrictions")

dbt will always connect to your data platform from the IP addresses specified in the [Regions & IP addresses](https://docs.getdbt.com/docs/cloud/about-cloud/access-regions-ip-addresses.md) page.

Be sure to allow traffic from these IPs in your firewall, and include them in any database grants.

Allowing these IP addresses only enables the connection to your data warehouse. However, you might want to send API requests from your restricted network to the dbt API. Using the dbt API requires allowing the `cloud.getdbt.com` subdomain. For more on the dbt architecture, see [Deployment architecture](https://docs.getdbt.com/docs/cloud/about-cloud/architecture.md).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
