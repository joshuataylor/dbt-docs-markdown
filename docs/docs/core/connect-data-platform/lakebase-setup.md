# Databricks Lakebase setup

* **Maintained by**:
  <!-- -->
  dbt Labs
* **Authors**:
  <!-- -->
  dbt Labs
* **GitHub repo**: [dbt-labs/dbt-adapters](https://github.com/dbt-labs/dbt-adapters) [![](https://img.shields.io/github/stars/dbt-labs/dbt-adapters?style=for-the-badge)](https://github.com/dbt-labs/dbt-adapters)
* **PyPI package**: `dbt-postgres` [![](https://badge.fury.io/py/dbt-postgres.svg)](https://badge.fury.io/py/dbt-postgres)
* **Slack channel**: [#db-postgres](https://getdbt.slack.com/archives/C0172G2E273)
* **Supported dbt Core version**:
  <!-- -->
  v1.0.0
  <!-- -->
  and newer
* **dbt support**:
  <!-- -->
  Supported
* **Minimum data platform version**:
  <!-- -->
  ?

## Installing <!-- -->dbt-postgres

Use `pip` to install the adapter. Before 1.8, installing the adapter would automatically install `dbt-core` and any additional dependencies. Beginning in 1.8, installing an adapter does not automatically install `dbt-core`. This is because adapters and dbt Core versions have been decoupled from each other so we no longer want to overwrite existing dbt-core installations. Use the following command for installation:

`python -m pip install dbt-core dbt-postgres`

## Configuring <!-- -->dbt-postgres<!-- -->

For <!-- -->Lakebase<!-- -->-specific configuration, please refer to [Lakebase<!-- --> configs.](https://docs.getdbt.com/reference/resource-configs/postgres-configs.md)

## Profile Configuration[​](#profile-configuration "Direct link to Profile Configuration")

Databricks Lakebase targets are configured exactly the same as [Postgres targets](https://docs.getdbt.com/docs/core/connect-data-platform/postgres-setup.md#profile-configuration).

Use these key parameters to connect to Databricks Lakebase:

* `host name`: Found in **Databricks** > **Compute** > **Database instances** > **Connect with PSQL** using the format `instance-123abcdef456.database.cloud.databricks.com`
* `database name`: Use `databricks_postgres` by default
* Authentication: dbt-postgres only supports username/password. You can generate a username/password by [enabling Native Postgres Role Login](https://docs.databricks.com/aws/en/oltp/oauth?language=UI#authenticate-with-databricks-identities) and use the role name as the username. To learn more about managing the Postgres roles and privileges, check out the [docs](https://docs.databricks.com/aws/en/oltp/pg-roles#create-postgres-roles-and-grant-privileges-for-databricks-identities).

Alternatively you can [generate an OAuth token](https://docs.databricks.com/aws/en/oltp/oauth?language=UI#authenticate-with-databricks-identities) that will need to be refreshed every hour to use with your Databricks username.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
