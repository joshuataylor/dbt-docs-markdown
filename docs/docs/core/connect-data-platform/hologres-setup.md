# Hologres setup

* **Maintained by**:
  <!-- -->
  Alibaba Cloud Hologres Team
* **Authors**:
  <!-- -->
  Alibaba Cloud Hologres Team
* **GitHub repo**: [aliyun/dbt-hologres](https://github.com/aliyun/dbt-hologres) [![](https://img.shields.io/github/stars/aliyun/dbt-hologres?style=for-the-badge)](https://github.com/aliyun/dbt-hologres)
* **PyPI package**: `dbt-alibaba-cloud-hologres` [![](https://badge.fury.io/py/dbt-alibaba-cloud-hologres.svg)](https://badge.fury.io/py/dbt-alibaba-cloud-hologres)
* **Slack channel**:[]()
* **Supported dbt Core version**:
  <!-- -->
  v1.8.0
  <!-- -->
  and newer
* **dbt support**:
  <!-- -->
  Not Supported
* **Minimum data platform version**:

## Installing <!-- -->dbt-alibaba-cloud-hologres

Use `pip` to install the adapter. Before 1.8, installing the adapter would automatically install `dbt-core` and any additional dependencies. Beginning in 1.8, installing an adapter does not automatically install `dbt-core`. This is because adapters and dbt Core versions have been decoupled from each other so we no longer want to overwrite existing dbt-core installations. Use the following command for installation:

`python -m pip install dbt-core dbt-alibaba-cloud-hologres`

## Configuring <!-- -->dbt-alibaba-cloud-hologres<!-- -->

For <!-- -->Hologres<!-- -->-specific configuration, please refer to [Hologres<!-- --> configs.](https://docs.getdbt.com/reference/resource-configs/no-configs.md)

## Connecting to Hologres with **dbt-alibaba-cloud-hologres**[​](#connecting-to-hologres-with-dbt-alibaba-cloud-hologres "Direct link to connecting-to-hologres-with-dbt-alibaba-cloud-hologres")

`dbt-alibaba-cloud-hologres` enables dbt to work with Alibaba Cloud Hologres, a real-time data warehouse compatible with PostgreSQL.

Check out the dbt profile configuration below for details.

\~/.dbt/profiles.yml

```yaml
dbt-alibaba-cloud-hologres: # this needs to match the profile in your dbt_project.yml file
  target: dev
  outputs:
    dev:
      type: hologres
      host: HOST_NAME
      port: 80
      user: USER_NAME
      password: PASSWORD
      database: DATABASE_NAME
      schema: SCHEMA_NAME
      threads: 4
```

### Connection parameters[​](#connection-parameters "Direct link to Connection parameters")

Currently it supports the following parameters:

| **Field**          | **Description**                                                                                | Required? | **Default**              | **Example**                       |
| ------------------ | ---------------------------------------------------------------------------------------------- | --------- | ------------------------ | --------------------------------- |
| `type`             | Specifies the type of database connection; must be set to "hologres" for Hologres connections. | Required  | -                        | `hologres`                        |
| `host`             | The endpoint hostname for connecting to Hologres instance.                                     | Required  | -                        | `hgxxx-xxx.hologres.aliyuncs.com` |
| `port`             | Port number for Hologres connection.                                                           | Optional  | `80`                     | `80`                              |
| `user`             | The username for authentication with Hologres (case-sensitive).                                | Required  | -                        | `AccessKey ID`                    |
| `password`         | The password for authentication with Hologres (case-sensitive).                                | Required  | -                        | `AccessKey Secret`                |
| `database`         | The name of your Hologres database.                                                            | Required  | -                        | `my_database`                     |
| `schema`           | The default schema that the models will use in Hologres (use empty string "" if not needed).   | Required  | -                        | `public`                          |
| `threads`          | Number of threads for parallel execution.                                                      | Optional  | `1`                      | `4`                               |
| `connect_timeout`  | Connection timeout in seconds.                                                                 | Optional  | `10`                     | `10`                              |
| `sslmode`          | SSL mode for the connection.                                                                   | Optional  | `disable`                | `disable`                         |
| `application_name` | Application identifier for connection tracking.                                                | Optional  | `dbt_hologres_{version}` | `my_dbt_app`                      |
| `retries`          | Number of connection retries.                                                                  | Optional  | `1`                      | `3`                               |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Authentication configuration[​](#authentication-configuration "Direct link to Authentication configuration")

`dbt-alibaba-cloud-hologres` uses the standard PostgreSQL-compatible authentication mechanism with username and password (Access Key). Hologres supports using Alibaba Cloud AccessKey or RAM user credentials for authentication.

### Access key[​](#access-key "Direct link to Access key")

You can authenticate using your Alibaba Cloud account credentials. For security reasons, it is recommended to create a RAM sub-account with appropriate permissions rather than using the primary account AccessKey.

```yaml
jaffle_shop: # this needs to match the profile in your dbt_project.yml file
  target: dev
  outputs:
    dev:
      type: hologres
      host: hgxxx-cn-shanghai.hologres.aliyuncs.com  # Replace with your Hologres endpoint
      port: 80
      user: your_access_key_id  # Replace with your AccessKeyId
      password: your_access_key_secret  # Replace with your AccessKeySecret
      database: my_database  # Replace with your database name
      schema: public  # Replace with your schema name
      threads: 4
      connect_timeout: 10
      sslmode: disable
```

### Important notes[​](#important-notes "Direct link to Important notes")

1. **Case sensitivity**: Hologres usernames and passwords are case-sensitive. Make sure to enter them exactly as configured.

2. **Default port**: The default port for Hologres is `80`, which is different from the standard PostgreSQL port `5432`.

3. **SSL mode**: SSL is disabled by default for Hologres connections. You can enable it by setting `sslmode` to an appropriate value if required.

## Testing your connection[​](#testing-your-connection "Direct link to Testing your connection")

After configuring your `profiles.yml`, you can verify your connection by running:

```bash
dbt debug
```

This [command](https://docs.getdbt.com/reference/commands/debug.md) will test the connection to your Hologres instance and report any configuration issues.

## Hologres-specific features[​](#hologres-specific-features "Direct link to Hologres-specific features")

### Dynamic tables in Hologres[​](#dynamic-tables-in-hologres "Direct link to Dynamic tables in Hologres")

Dynamic tables are Hologres's implementation of materialized views with automatic refresh. When refreshing data, multiple modes are supported, including "full" (full mode) and "incremental" (incremental mode). For more information, please refer to the [reference manual](https://www.alibabacloud.com/help/en/hologres/user-guide/introduction-to-dynamic-table). You can configure them in your dbt models:

```yaml
models:
  my_model:
    materialized: dynamic_table
    freshness: "30 minutes"
    auto_refresh_mode: auto
    computing_resource: serverless
```

Supported configurations for Dynamic tables:

| **Configuration**    | **Description**                           | **Example values**                    |
| -------------------- | ----------------------------------------- | ------------------------------------- |
| `freshness`          | Data freshness requirement.               | `"30 minutes"`, `"1 hours"`           |
| `auto_refresh_mode`  | Refresh mode for the dynamic table.       | `auto`, `incremental`, `full`         |
| `computing_resource` | Computing resource to use for refreshing. | `serverless`, `local`, warehouse name |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Incremental models with dbt[​](#incremental-models-with-dbt "Direct link to Incremental models with dbt")

`dbt-alibaba-cloud-hologres` supports multiple incremental strategies:

* `append`: Simply append new records
* `delete+insert`: Delete matching records and insert new ones
* `merge`: Use MERGE statement for upsert operations
* `microbatch`: Process data in small batches

### Constraints[​](#constraints "Direct link to Constraints")

Full support for database constraints including:

* Primary keys
* Not null constraints

### Table properties[​](#table-properties "Direct link to Table properties")

Hologres supports the following table properties. For full details, see the [developer reference documentation](https://www.alibabacloud.com/help/en/hologres/developer-reference/create-tables).

| Property                      | Best practices                                                               |
| ----------------------------- | ---------------------------------------------------------------------------- |
| `orientation`                 | Use `column` for olap workloads and `row` for key-value queries              |
| `distribution_key`            | Choose frequently joined or grouped columns; prefer a single column          |
| `clustering_key`              | Use for range filter columns; max 3 columns; follow the left-match principle |
| `event_time_column`           | Set for time-series data (timestamp columns)                                 |
| `bitmap_columns`              | Use for equality filters                                                     |
| `dictionary_encoding_columns` | Use for low-cardinality string columns                                       |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## References[​](#references "Direct link to References")

* [dbt-alibaba-cloud-hologres GitHub repository](https://github.com/aliyun/dbt-hologres)
* [Hologres documentation](https://www.alibabacloud.com/help/en/hologres/)
* [Hologres dynamic table guide](https://www.alibabacloud.com/help/en/hologres/user-guide/introduction-to-dynamic-table)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
