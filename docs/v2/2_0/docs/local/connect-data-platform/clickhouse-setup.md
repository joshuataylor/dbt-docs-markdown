(Applies to dbt v2.0 and later)

# Connect ClickHouse to dbt v2 [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Local development

The ClickHouse adapter for the dbt v2 connects to [ClickHouse](https://clickhouse.com) over HTTP or HTTPS. It supports self-managed single-node ClickHouse and [ClickHouse Cloud](https://clickhouse.com/cloud).

## Installing dbt

The ClickHouse adapter is built into v2. To get started, [install dbt](../install-dbt.md).

For connection examples and profile settings, refer to [Connecting to ClickHouse](#connecting-to-clickhouse).

## Authentication

dbt v2 authenticates to ClickHouse with a username and password. Set `secure: true` in your profile to connect over HTTPS (default port 8443), or leave it unset to connect over plain HTTP (default port 8123). ClickHouse Cloud requires `secure: true`.

## Warehouse permissions

The ClickHouse user that the dbt v2 connects as must be able to run dbt workloads in the target database and read the system tables used for introspection.

### Required ClickHouse objects

Before connecting, these objects must exist or be accessible:

| Object                                                           | Purpose                                                                                          |
| ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| Service (ClickHouse Cloud) or server (self-managed, single node) | Compute resource                                                                                 |
| Database                                                         | Target database. ClickHouse has no separate schema level, so the dbt `schema` maps to a database |
| User                                                             | Database user for authentication                                                                 |

### Core permissions

The following permissions are required for fundamental dbt features:

| Permission                                         | Object           | Purpose                                                                                                                                                                      |
| -------------------------------------------------- | ---------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `SELECT`                                           | Tables and views | Read data                                                                                                                                                                    |
| `INSERT`                                           | Tables           | Load models, seeds, and snapshots                                                                                                                                            |
| `ALTER`                                            | Tables           | Schema changes (`on_schema_change`), indexes and projections, comments (`persist_docs`), `REPLACE PARTITION` (`insert_overwrite`), and lightweight deletes (`delete+insert`) |
| `TRUNCATE`                                         | Tables           | Full-refresh seeds                                                                                                                                                           |
| `CREATE TABLE`, `CREATE VIEW`, `CREATE DICTIONARY` | Database         | Create materializations, including the intermediate and backup relations used for atomic rebuilds (`EXCHANGE TABLES`, `RENAME TABLE`)                                        |
| `DROP TABLE`, `DROP VIEW`, `DROP DICTIONARY`       | Database         | Drop or replace objects                                                                                                                                                      |

### Metadata operations

dbt v2 reads these ClickHouse system tables:

| System table            | Purpose                                                                          |
| ----------------------- | -------------------------------------------------------------------------------- |
| `system.tables`         | List relations, build the catalog, detect materialized views pointing at a table |
| `system.columns`        | Column metadata for the catalog and for schema-change detection                  |
| `system.databases`      | Check whether the target database exists                                         |
| `system.settings`       | Capability probes (lightweight deletes, `insert_distributed_sync`)               |
| `system.view_refreshes` | Validate refreshable materialized view dependencies                              |

ClickHouse filters system tables to the objects the user can access, so no separate grant is needed for them.

### Database management

Conditional permissions for database management:

| Permission        | Object | When required                                             |
| ----------------- | ------ | --------------------------------------------------------- |
| `CREATE DATABASE` | Server | Auto-create the target database when it doesn't exist yet |

## Limitations

The ClickHouse adapter for dbt v2 is in beta. Expect some minor bugs, and avoid using it in production environments for now. Some features available in the `dbt-clickhouse` adapter for dbt v1 are not yet supported.

The ClickHouse adapter is under active development. If you encounter an issue, please [open it in dbt-core](https://github.com/dbt-labs/dbt-core/issues/new) and add the `adapter:clickhouse` label.

### What works today

On single-node ClickHouse and on ClickHouse Cloud (compatible with multi-node clusters):

* All materializations: table, view, incremental (all strategies and `on_schema_change`), materialized view (including refreshable), dictionary, snapshot, seed, and ephemeral
* Contracts and constraints, model and query `settings`, projections and indexes
* Data tests, unit tests, catalog generation, and the `s3` table function

### Not yet supported

* Self-managed clusters that set `cluster:` in the profile aren't supported yet. `ON CLUSTER` isn't emitted in the data definition language (DDL) instructions, so Replicated engines and the `distributed_table` and `distributed_incremental` materializations don't work.
* The `grants` config, and the `dbt clone` and `dbt source freshness` commands, aren't supported yet.
* Smaller gaps remain: `query-comment: null` isn't honored, run results don't include the ClickHouse `query_id`, and `persist_docs` descriptions that contain `;` fail.
* SQL comprehension features (static analysis and the rest of dbt's SQL intelligence) aren't available yet. Support is coming soon.
* Minor issues may still be present in general ClickHouse functionality.

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
