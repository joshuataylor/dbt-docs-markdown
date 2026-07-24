# Connect Redshift to dbt Core

Local developmentⓘ

[Fusion compatible](https://docs.getdbt.com/docs/local/connect-data-platform/redshift-setup.md?version=2 "Fusion compatible") connection also available.

* **Maintained by**:
  <!-- -->
  dbt Labs
* **Authors**:
  <!-- -->
  dbt maintainers
* **GitHub repo**: [dbt-labs/dbt-adapters](https://github.com/dbt-labs/dbt-adapters) [![](https://img.shields.io/github/stars/dbt-labs/dbt-adapters?style=for-the-badge)](https://github.com/dbt-labs/dbt-adapters)
* **PyPI package**: `dbt-redshift` [![](https://badge.fury.io/py/dbt-redshift.svg)](https://badge.fury.io/py/dbt-redshift)
* **Slack channel**: [#db-redshift](https://getdbt.slack.com/archives/C01DRQ178LQ)
* **Supported dbt Core version**:
  <!-- -->
  v0.10.0
  <!-- -->
  and newer
* **dbt support**:
  <!-- -->
  Supported
* **Minimum data platform version**:
  <!-- -->
  n/a

## Installing <!-- -->dbt-redshift

Use `pip` to install the adapter. Use the following command for installation:

`python -m pip install dbt-redshift`

## Configuring <!-- -->dbt-redshift<!-- -->

For <!-- -->Redshift<!-- -->-specific configuration, please refer to [Redshift<!-- --> configs.](https://docs.getdbt.com/reference/resource-configs/redshift-configs.md)

## Configurations[​](#configurations "Direct link to Configurations")

| Profile field                                                                                                                                           | Example                                                                                                  | Description                                                                                                                                                                                                                      |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `type`                                                                                                                                                  | redshift                                                                                                 | The type of data warehouse you are connecting to                                                                                                                                                                                 |
| `host`                                                                                                                                                  | `hostname.region.redshift.amazonaws.com` or `workgroup.account.region.redshift-serverless.amazonaws.com` | Host of cluster                                                                                                                                                                                                                  |
| `port`                                                                                                                                                  | 5439                                                                                                     | Port for your Redshift environment                                                                                                                                                                                               |
| `dbname`                                                                                                                                                | my\_db                                                                                                   | Database name                                                                                                                                                                                                                    |
| `schema`                                                                                                                                                | my\_schema                                                                                               | Schema name                                                                                                                                                                                                                      |
| `connect_timeout`                                                                                                                                       | 30                                                                                                       | Number of seconds before the connection times out. Default is `None`                                                                                                                                                             |
| `sslmode`                                                                                                                                               | prefer                                                                                                   | optional, set the sslmode to connect to the database. Defaults to `prefer`, which will use 'verify-ca' to connect. For more information on `sslmode`, see Redshift note below                                                    |
| `role`                                                                                                                                                  | None                                                                                                     | Optional, user identifier of the current session                                                                                                                                                                                 |
| `autocreate`                                                                                                                                            | false                                                                                                    | Optional, default `False`. Creates user if they do not exist                                                                                                                                                                     |
| `db_groups`                                                                                                                                             | \['ANALYSTS']                                                                                            | Optional. A list of existing database group names that the DbUser joins for the current session                                                                                                                                  |
| `ra3_node`                                                                                                                                              | true                                                                                                     | Optional, default `False`. Enables cross-database sources. Kept for backward compatibility; use `datasharing` for new projects instead.                                                                                          |
| `datasharing` [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles") | true                                                                                                     | Optional, default `False`. Enables cross-database and cross-cluster access for [Redshift Datasharing](https://docs.aws.amazon.com/redshift/latest/dg/datashare-overview.html). Available in `dbt-redshift` v1.11.0rc1 and later. |
| `autocommit`                                                                                                                                            | true                                                                                                     | Optional, default `True`. Enables autocommit after each statement                                                                                                                                                                |
| `retries`                                                                                                                                               | 1                                                                                                        | Number of retries (on each statement)                                                                                                                                                                                            |
| `retry_all`                                                                                                                                             | true                                                                                                     | Allows dbt to retry all statements in a query                                                                                                                                                                                    |
| `tcp_keepalive`                                                                                                                                         | true                                                                                                     | Allows dbt to prevent idle connections from being dropped by intermediate firewalls or load-balancers                                                                                                                            |
| `tcp_keepalive_idle`                                                                                                                                    | 200                                                                                                      | Number of seconds of inactivity before the first keep-alive probe is sent                                                                                                                                                        |
| `tcp_keepalive_interval`                                                                                                                                | 200                                                                                                      | Number of seconds of inactivity before the next probe is sent                                                                                                                                                                    |
| `tcp_keepalive_count`                                                                                                                                   | 5                                                                                                        | Number of times probes will be sent                                                                                                                                                                                              |
| `drop_without_cascade`                                                                                                                                  | false                                                                                                    | Optional, default `False`. Omits `CASCADE` from `DROP TABLE/VIEW/MATERIALIZED VIEW` statements. Available in `dbt-redshift` v1.11.0rc3 and later.                                                                                |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

For your tcp\_keepalive inputs, we recommend taking a look at the [Redshift documentation](https://docs.aws.amazon.com/redshift/latest/mgmt/troubleshooting-connections.html) for more information on the right configuration for you.

## Authentication Parameters[​](#authentication-parameters "Direct link to Authentication Parameters")

The authentication methods that dbt Core supports on Redshift are:

* `Database` — Password-based authentication (default, will be used if `method` is not provided)
* `IAM User` — IAM User authentication via AWS Profile

Click on one of these authentication methods for further details on how to configure your connection profile. Each tab also includes an example `profiles.yml` configuration file for you to review.

* Database
* IAM User via AWS Profile (Core)

The following table contains the parameters for the database (password-based) connection method.

| Profile field | Example   | Description                                                |
| ------------- | --------- | ---------------------------------------------------------- |
| `method`      | database  | Leave this parameter unconfigured, or set this to database |
| `user`        | username  | Account username to log into your cluster                  |
| `password`    | password1 | Password for authentication                                |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

<br />

#### Example profiles.yml for database authentication[​](#example-profilesyml-for-database-authentication "Direct link to Example profiles.yml for database authentication")

\~/.dbt/profiles.yml

```yaml
company-name:
  target: dev
  outputs:
    dev:
      type: redshift
      host: hostname.region.redshift.amazonaws.com
      user: username
      password: password1
      dbname: analytics
      schema: analytics
      port: 5439

      # Optional Redshift configs:
      sslmode: prefer
      role: None
      ra3_node: true
      datasharing: true
      autocommit: true
      threads: 4
      connect_timeout: None
```

The following table lists the authentication parameters to use IAM authentication.

To set up a Redshift profile using IAM Authentication, set the `method` parameter to `iam` as shown below. Note that a password is not required when using IAM Authentication. For more information on this type of authentication, consult the [Redshift Documentation](https://docs.aws.amazon.com/redshift/latest/mgmt/generating-user-credentials.html) and [boto3 docs](https://boto3.amazonaws.com/v1/documentation/api/latest/reference/services/redshift.html#Redshift.Client.get_cluster_credentials) on generating user credentials with IAM Auth.

If you receive the "You must specify a region" error when using IAM Authentication, then your aws credentials are likely misconfigured. Try running `aws configure` to set up AWS access keys, and pick a default region. If you have any questions, please refer to the official AWS documentation on [Configuration and credential file settings](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html).

| Profile field | Example     | Description                                                                     |
| ------------- | ----------- | ------------------------------------------------------------------------------- |
| `method`      | IAM         | use IAM to authenticate via IAM User authentication                             |
| `iam_profile` | analyst     | dbt will use the specified profile from your \~/.aws/config file                |
| `cluster_id`  | cluster\_id | Required for IAM authentication only for provisoned cluster, not for Serverless |
| `user`        | username    | User querying the database, ignored for Serverless (but field still required)   |
| `region`      | us-east-1   | Region of your Redshift instance                                                |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

<br />

#### Example profiles.yml for IAM[​](#example-profilesyml-for-iam "Direct link to Example profiles.yml for IAM")

\~/.dbt/profiles.yml

```yaml
  my-redshift-db:
  target: dev
  outputs:
    dev:
      type: redshift
      method: iam
      cluster_id: CLUSTER_ID
      host: hostname.region.redshift.amazonaws.com
      user: alice
      iam_profile: analyst
      region: us-east-1
      dbname: analytics
      schema: analytics
      port: 5439

      # Optional Redshift configs:
      threads: 4
      connect_timeout: None 
      retries: 1 
      role: None
      sslmode: prefer
      ra3_node: true
      datasharing: true
      autocommit: true
      autocreate: true
      db_groups: ['ANALYSTS']
```

#### Specifying an IAM Profile[​](#specifying-an-iam-profile "Direct link to Specifying an IAM Profile")

When the `iam_profile` configuration is set, dbt will use the specified profile from your `~/.aws/config` file instead of using the profile name `default`

## Redshift notes[​](#redshift-notes "Direct link to Redshift notes")

### `sslmode` change[​](#sslmode-change "Direct link to sslmode-change")

Before dbt-redshift 1.5, `psycopg2` was used as the driver. `psycopg2` accepts `disable`, `prefer`, `allow`, `require`, `verify-ca`, `verify-full` as valid inputs of `sslmode`, and does not have an `ssl` parameter, as indicated in PostgreSQL [doc](https://www.postgresql.org/docs/current/libpq-connect.html#LIBPQ-CONNSTRING:~:text=%2Dencrypted%20connection.-,sslmode,-This%20option%20determines).

In dbt-redshift 1.5, we switched to using `redshift_connector`, which accepts `verify-ca`, and `verify-full` as valid `sslmode` inputs, and has a `ssl` parameter of `True` or `False`, according to redshift [doc](https://docs.aws.amazon.com/redshift/latest/mgmt/python-configuration-options.html#:~:text=parameter%20is%20optional.-,sslmode,-Default%20value%20%E2%80%93%20verify).

For backward compatibility, dbt-redshift now supports valid inputs for `sslmode` in `psycopg2`. We've added conversion logic mapping each of `psycopg2`'s accepted `sslmode` values to the corresponding `ssl` and `sslmode` parameters in `redshift_connector`.

The table below details accepted `sslmode` parameters and how the connection will be made according to each option:

| `sslmode` parameter | Expected behavior in dbt-redshift         | Actions behind the scenes                  |
| ------------------- | ----------------------------------------- | ------------------------------------------ |
| disable             | Connection will be made without using ssl | Set `ssl` = False                          |
| allow               | Connection will be made using verify-ca   | Set `ssl` = True & `sslmode` = verify-ca   |
| prefer              | Connection will be made using verify-ca   | Set `ssl` = True & `sslmode` = verify-ca   |
| require             | Connection will be made using verify-ca   | Set `ssl` = True & `sslmode` = verify-ca   |
| verify-ca           | Connection will be made using verify-ca   | Set `ssl` = True & `sslmode` = verify-ca   |
| verify-full         | Connection will be made using verify-full | Set `ssl` = True & `sslmode` = verify-full |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

When a connection is made using `verify-ca`, will look for the CA certificate in `~/redshift-ca-bundle.crt`.

For more details on sslmode changes, our design choices, and reasoning — please refer to the [PR pertaining to this change](https://github.com/dbt-labs/dbt-redshift/pull/439).

### `autocommit` parameter[​](#autocommit-parameter "Direct link to autocommit-parameter")

The [autocommit mode](https://www.psycopg.org/docs/connection.html#connection.autocommit) is useful to execute commands that run outside a transaction. Connection objects used in Python must have `autocommit = True` to run operations such as `CREATE DATABASE`, and `VACUUM`. `autocommit` is off by default in `redshift_connector`, but we've changed this default to `True` to ensure certain macros run successfully in your dbt project.

If desired, you can define a separate target with `autocommit=True` as such:

\~/.dbt/profiles.yml

```yaml
profile-to-my-RS-target:
  target: dev
  outputs:
    dev:
      type: redshift
      ...
      autocommit: False
      
  
  profile-to-my-RS-target-with-autocommit-enabled:
  target: dev
  outputs:
    dev:
      type: redshift
      ...
      autocommit: True
```

To run certain macros with autocommit, load the profile with autocommit using the `--profile` flag. For more context, please refer to this [PR](https://github.com/dbt-labs/dbt-redshift/pull/475/files).

### `datasharing` [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[​](#datasharing- "Direct link to datasharing-")

Previously, the Redshift adapter used PostgreSQL-compatible catalog tables (for example, `pg_*`, `information_schema`) for metadata operations such as listing relations, schemas, and columns. These tables only surface objects within the currently connected database, which prevents cross-database operations needed for [Redshift Datasharing](https://docs.aws.amazon.com/redshift/latest/dg/datashare-overview.html).

Starting `dbt-redshift` v1.11.0rc1, you can set `datasharing: true` in your `profiles.yml` to enable cross-database and cross-cluster access. When enabled, `dbt-redshift` switches metadata queries to Redshift's native `SHOW` system commands. You can then materialize models into a database or cluster other than the one specified in your profile using `{{ config(database='other_db') }}`.

Example configuration:

profiles.yml

```yml
company-name:
  target: dev
  outputs:
    dev:
      type: redshift
      host: hostname.region.redshift.amazonaws.com
      ...
      datasharing: true  # default: false
```

Once enabled, you can materialize a model into a different database by setting `database` in the model config. For example:

```sql
{{ config(database='other_db') }}

select * from {{ ref('my_model') }}
```

The following macros switch to `SHOW` commands when `datasharing: true`:

| Macro                                 | Without `datasharing`               | With `datasharing`                                 |
| ------------------------------------- | ----------------------------------- | -------------------------------------------------- |
| `list_relations_without_caching`      | `information_schema.tables`         | `SHOW TABLES FROM SCHEMA`                          |
| `list_schemas`, `check_schema_exists` | `pg_namespace`                      | `SHOW SCHEMAS FROM DATABASE`                       |
| `get_columns_in_relation`             | `information_schema.columns`        | `SHOW COLUMNS FROM TABLE`                          |
| Catalog queries                       | `pg_class`, `pg_tables`, `pg_views` | `SHOW TABLES FROM SCHEMA` + `SVV_REDSHIFT_COLUMNS` |
| `get_relation_last_modified`          | `information_schema.tables`         | `SHOW TABLES FROM SCHEMA`                          |
| Grants                                | `pg_user`, `has_table_privilege()`  | `SHOW GRANTS ON TABLE`                             |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

`ra3_node: true` also enables this behavior and is supported for backward compatibility. For new projects, use `datasharing: true` instead.

note

If you see `pg_*` queries running with `datasharing` enabled, this is not necessarily a bug. Only the macros listed above are migrated — some macros remain on `pg_*` by design (for example, `get_relations` for dependency tracking and `list_function_relations_without_caching` for function discovery). Custom macro overrides in your project are not affected. To confirm whether a `pg_*` query is expected, check which macro is being overridden and which metadata operation is running.

The following limitations apply when using `datasharing`:

* Creating views (including materialized views) in another database is not supported.
* Cross-database grants on objects are not supported.
* Source freshness checks can have a lag of up to 5 minutes.
* Metadata queries are limited to 10,000 rows. If a database has more than 10,000 schemas, or a schema has more than 10,000 tables, dbt runs can result in unexpected scenarios.
* Cross-database writes require the `SNAPSHOT` transaction isolation level.
* For views that reference tables in another database, define them as [late-binding views](https://docs.getdbt.com/reference/resource-configs/redshift-configs.md#late-binding-views).

### Deprecated `profile` parameters in 1.5[​](#deprecated-profile-parameters-in-15 "Direct link to deprecated-profile-parameters-in-15")

* `iam_duration_seconds`

* `keepalives_idle`

### `drop_without_cascade`[​](#drop_without_cascade "Direct link to drop_without_cascade")

Set `drop_without_cascade: true` to omit `CASCADE` from `DROP TABLE`, `DROP VIEW`, and `DROP MATERIALIZED VIEW` statements. Use this when your project has no downstream dependents (for example, it uses only unbound views) and you want to avoid the overhead of resolving the `CASCADE` dependency graph on every drop for large clusters.

info

This option is intended for projects with no downstream dependents. If a dependent object exists and `CASCADE` is omitted, Redshift raises an error.

### `sort` and `dist` keys[​](#sort-and-dist-keys "Direct link to sort-and-dist-keys")

Where possible, dbt enables the use of `sort` and `dist` keys. See the section on [Redshift specific configurations](https://docs.getdbt.com/reference/resource-configs/redshift-configs.md).

#### retries[​](#retries "Direct link to retries")

If `dbt-redshift` encounters an operational error or timeout when opening a new connection, it will retry up to the number of times configured by `retries`. If set to 2+ retries, dbt will wait 1 second before retrying. The default value is 1 retry. If set to 0, dbt will not retry at all.
