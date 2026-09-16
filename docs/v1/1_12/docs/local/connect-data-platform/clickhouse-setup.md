(Applies to dbt v1.99 and earlier)

# Connect ClickHouse to dbt v1

Local development[Fusion compatible](./clickhouse-setup.md?version=2 "Fusion compatible")

A dbt v2 connection is also available.

Community plugin

Some core functionality may be limited. If you're interested in contributing, check out the source code for each repository listed below.

* **Maintained by**: Community
* **Authors**: Geoff Genz & Bentsi Leviav
* **GitHub repo**: [ClickHouse/dbt-clickhouse](https://github.com/ClickHouse/dbt-clickhouse) [![](https://img.shields.io/github/stars/ClickHouse/dbt-clickhouse?style=for-the-badge)](https://github.com/ClickHouse/dbt-clickhouse)
* **PyPI package**: `dbt-clickhouse` [![](https://badge.fury.io/py/dbt-clickhouse.svg)](https://badge.fury.io/py/dbt-clickhouse)
* **Slack channel**: [#db-clickhouse](https://getdbt.slack.com/archives/C01DRQ178LQ)
* **Supported dbt version**: v0.19.0 and newer
* **dbt support**: Supported
* **Minimum data platform version**: n/a

## Installing dbt-clickhouse

Use `pip` to install the adapter. Use the following command for installation:

`python -m pip install dbt-clickhouse`

## Configuring dbt-clickhouse

For ClickHouse-specific configuration, please refer to [ClickHouse configs.](../../../reference/resource-configs/clickhouse-configs.md)

## Connecting to ClickHouse

To connect to ClickHouse from dbt, you'll need to add a [profile](../profiles.yml.md) to your `profiles.yml` configuration file. Follow the reference configuration below to set up a ClickHouse profile:

profiles.yml

```yaml
clickhouse-service:
  target: dev
  outputs:
    dev:
      type: clickhouse
      schema: [ default ]  # ClickHouse database for dbt models

      # optional
      host: [ <your-clickhouse-host> ]  # Your clickhouse cluster url for example, abc123.clickhouse.cloud. Defaults to `localhost`.
      port: [ 8123 ]  # Defaults to 8123, 8443, 9000, 9440 depending on the secure and driver settings 
      user: [ default ]  # User for all database operations
      password: [ <empty string> ]  # Password for the user
      secure: [ False ]  # Use TLS (native protocol) or HTTPS (http protocol). Must be set to true for ClickHouse Cloud.
```

For a complete list of configuration options, refer to the [ClickHouse documentation](https://clickhouse.com/docs/integrations/dbt).

### Create a dbt project

You can now use this profile in one of your existing projects or create a new one using:

```sh
dbt init project_name
```

Navigate to the `project_name` directory and update your `dbt_project.yml` file to use the profile you configured to connect to ClickHouse.

```yaml
profile: 'clickhouse-service'
```

### Test connection

Execute `dbt debug` with the CLI tool to confirm whether dbt is able to connect to ClickHouse. Confirm the response includes `Connection test: [OK connection ok]`, indicating a successful connection.

(Applies to dbt v1.99 and earlier)

## Supported features

### dbt features

| Type                  | Supported? | Details                    |
| --------------------- | ---------- | -------------------------- |
| Contracts             | ✅         |                            |
| Docs generate         | ✅         |                            |
| Most dbt-utils macros | ✅         | (now included in dbt-core) |
| Seeds                 | ✅         |                            |
| Sources               | ✅         |                            |
| Snapshots             | ✅         |                            |
| Tests                 | ✅         |                            |

### Materializations

| Type                                    | Supported?       | Details                                                                                                                                                                                                                                                                                                |
| --------------------------------------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Table                                   | ✅               | Creates a [table](https://clickhouse.com/docs/en/operations/system-tables/tables/).                                                                                                                                                                                                                    |
| View                                    | ✅               | Creates a [view](https://clickhouse.com/docs/en/sql-reference/table-functions/view/).                                                                                                                                                                                                                  |
| Incremental                             | ✅               | Creates a table if it doesn't exist, and then writes only updates to it.                                                                                                                                                                                                                               |
| Microbatch incremental                  | ✅               |                                                                                                                                                                                                                                                                                                        |
| Ephemeral materialization               | ✅               | Creates a ephemeral/CTE materialization. This model is internal to dbt and does not create any database objects.                                                                                                                                                                                       |
| Materialized View                       | ✅, Experimental | Creates a [materialized view](https://clickhouse.com/docs/en/materialized-view).                                                                                                                                                                                                                       |
| Distributed table materialization       | ✅, Experimental | Creates a [distributed table](https://clickhouse.com/docs/en/engines/table-engines/special/distributed).                                                                                                                                                                                               |
| Distributed incremental materialization | ✅, Experimental | Incremental model based on the same idea as distributed table. Note that not all strategies are supported. For more information, refer to the [distributed incremental materialization docs](https://github.com/ClickHouse/dbt-clickhouse?tab=readme-ov-file#distributed-incremental-materialization). |
| Dictionary materialization              | ✅, Experimental | Creates a [dictionary](https://clickhouse.com/docs/en/engines/table-engines/special/dictionary).                                                                                                                                                                                                       |

**Note**: Community-developed features are labeled as experimental. Despite this designation, many of these features, like materialized views, are widely adopted and successfully used in production environments.

## Documentation

Refer to the [ClickHouse documentation](https://clickhouse.com/docs/integrations/dbt) for more details on using the `dbt-clickhouse` adapter to manage your data model.

## Contributing

We welcome contributions from the community to help improve the `dbt-ClickHouse` adapter. Whether you're fixing a bug, adding a new feature, or enhancing the documentation, your efforts are greatly appreciated!

Please take a moment to read our [Contribution Guide](https://github.com/ClickHouse/dbt-clickhouse/blob/main/CONTRIBUTING.md) to get started. The guide provides detailed instructions on setting up your environment, running tests, and submitting pull requests.
