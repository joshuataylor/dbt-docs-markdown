# DuckDB configurations

These configurations are specific to `dbt-duckdb`. For profile setup and connection options, refer to [Connect DuckDB](https://docs.getdbt.com/docs/local/connect-data-platform/duckdb-setup.md). For general dbt concepts, refer to [Materializations](https://docs.getdbt.com/docs/build/materializations.md) and [Incremental models](https://docs.getdbt.com/docs/build/incremental-models.md).

Some features require a minimum version of `dbt-duckdb`. Version requirements are noted inline throughout this page.

## Secrets manager[​](#secrets-manager "Direct link to Secrets manager")

Use the [DuckDB Secrets Manager](https://duckdb.org/docs/configuration/secrets_manager.html) to manage credentials for cloud storage. Configure the `secrets` field in your profile:

```yml
default:
  outputs:
    dev:
      type: duckdb
      path: /tmp/dbt.duckdb
      extensions:
        - httpfs
        - parquet
      secrets:
        - type: s3
          region: my-aws-region
          key_id: "{{ env_var('S3_ACCESS_KEY_ID') }}"
          secret: "{{ env_var('S3_SECRET_ACCESS_KEY') }}"
  target: dev
```

### Fetch credentials from context[​](#fetch-credentials-from-context "Direct link to Fetch credentials from context")

Instead of specifying credentials directly, you can use the `credential_chain` secret provider to use any supported AWS mechanism (for example, web identity tokens). Refer to the [DuckDB secret providers documentation](https://duckdb.org/docs/configuration/secrets_manager.html#secret-providers) for details.

```yml
secrets:
  - type: s3
    provider: credential_chain
```

### Scoped credentials by storage prefix[​](#scoped-credentials-by-storage-prefix "Direct link to Scoped credentials by storage prefix")

Secrets can be scoped so that different storage paths use different credentials:

```yml
secrets:
  - type: s3
    provider: credential_chain
    scope: [ "s3://bucket-in-eu-region", "s3://bucket-2-in-eu-region" ]
    region: "eu-central-1"
  - type: s3
    region: us-west-2
    scope: "s3://bucket-in-us-region"
```

When fetching a secret for a path, the secret scopes are compared to the path. In the case of multiple matching secrets, the longest prefix is chosen.

## Cloud storage with fsspec[​](#cloud-storage-with-fsspec "Direct link to Cloud storage with fsspec")

In `dbt-duckdb 1.4.1` and later, you can experimentally use DuckDB filesystems implemented via [fsspec](https://duckdb.org/docs/guides/python/filesystems.html). The `fsspec` library supports [a variety of cloud data storage systems](https://filesystem-spec.readthedocs.io/en/latest/api.html#other-known-implementations), including S3, GCS, and Azure Blob Storage.

To use an `fsspec` implementation, install the relevant Python modules and configure `filesystems` in your profile:

```yml
default:
  outputs:
    dev:
      type: duckdb
      path: /tmp/dbt.duckdb
      filesystems:
        - fs: s3
          anon: false
          key: "{{ env_var('S3_ACCESS_KEY_ID') }}"
          secret: "{{ env_var('S3_SECRET_ACCESS_KEY') }}"
          client_kwargs:
            endpoint_url: "http://localhost:4566"
  target: dev
```

Each entry must include an `fs` property that identifies the `fsspec` protocol to load (`s3`, `gcs`, `abfs`, etc.) and can include additional key-value pairs to configure that implementation.

## Arbitrary ATTACH options[​](#arbitrary-attach-options "Direct link to Arbitrary ATTACH options")

For the basic `attach` profile syntax, refer to [Connecting to DuckDB](https://docs.getdbt.com/docs/local/connect-data-platform/duckdb-setup.md#attaching-additional-databases). Use the `options` dictionary when you need to pass additional key-value pairs to DuckDB's `ATTACH` statement:

```yml
attach:
  - path: /tmp/db1.sqlite
    type: sqlite
    read_only: true
  - path: /tmp/special.duckdb
    options:
      cache_size: 1GB
      threads: 4
      enable_fsst: true
```

If you specify the same option in both a direct field (`type`, `secret`, `read_only`) and in the `options` dict, `dbt-duckdb` raises an error to prevent conflicts.

## DuckLake[​](#ducklake "Direct link to DuckLake")

[DuckLake](https://ducklake.select) is a table format that provides ACID transactions and time travel for DuckDB. You can use DuckLake with both local databases and MotherDuck.

### DuckLake on MotherDuck[​](#ducklake-on-motherduck "Direct link to DuckLake on MotherDuck")

In `dbt-duckdb 1.9.6` and later, you can connect to [hosted DuckLake on MotherDuck](https://motherduck.com/blog/ducklake-motherduck/) by creating a DuckLake database and setting `is_ducklake: true`.

To set up DuckLake on MotherDuck:

1. Create your DuckLake database in MotherDuck:

```sql
CREATE DATABASE my_ducklake
  (TYPE ducklake, DATA_PATH 's3://...')
```

2. Configure your profile:

```yml
default:
  outputs:
    dev:
      type: duckdb
      path: "md:my_db?motherduck_token={{ env_var('MOTHERDUCK_TOKEN') }}"
      attach:
        - path: "md:my_ducklake"
          is_ducklake: true
  target: dev
```

You must identify DuckLake must with `is_ducklake: true` so that dbt applies safe DDL operations.

For local DuckLake, use `ducklake:` in the path:

```yml
attach:
  - path: "ducklake:my_ducklake.ddb"
```

### DuckLake table partitioning[​](#ducklake-table-partitioning "Direct link to DuckLake table partitioning")

For DuckLake-backed tables (including MotherDuck-managed DuckLake), you can configure physical partitioning for `table` or `incremental` models using `partitioned_by`:

```sql
{{ config(materialized='table', partitioned_by=['year', 'month']) }}

select
  *,
  year(event_time) as year,
  month(event_time) as month
from {{ ref('upstream_model') }}
```

`partition_by` is accepted as an alias for `partitioned_by`. This setting is only applied for DuckLake relations; on non-DuckLake targets, it is ignored with a warning.

DuckLake applies partitioning using `ALTER TABLE ... SET PARTITIONED BY (...)`, and partitioning only affects new data. For first builds or full refreshes, `dbt-duckdb` creates an empty table, sets partitioning, then inserts data so the initial load is partitioned. Refer to the [DuckLake partitioning documentation](https://ducklake.select/docs/stable/duckdb/advanced_features/partitioning) for more details.

<!-- -->

## Incremental strategies[​](#incremental-strategies "Direct link to Incremental strategies")

`dbt-duckdb` supports the following strategies for incremental table models:

* [`append`](#append-strategy)
* [`delete+insert`](#deleteinsert-strategy)
* [`merge`](#merge-strategy)
* [`microbatch`](#microbatch-strategy)

### Append strategy[​](#append-strategy "Direct link to Append strategy")

| Configuration            | Type     | Default | Description                                          |
| ------------------------ | -------- | ------- | ---------------------------------------------------- |
| `incremental_predicates` | `<list>` | null    | SQL conditions to filter which records get appended. |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Delete+insert strategy[​](#deleteinsert-strategy "Direct link to Delete+insert strategy")

| Configuration            | Type                | Default | Description                                                |
| ------------------------ | ------------------- | ------- | ---------------------------------------------------------- |
| `unique_key`             | `<string>`/`<list>` | —       | Required. Columns used to identify records for deletion.   |
| `incremental_predicates` | `<list>`            | null    | SQL conditions to filter the delete and insert operations. |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Merge strategy[​](#merge-strategy "Direct link to Merge strategy")

The `merge` strategy requires DuckDB 1.4.0 or later and provides access to DuckDB's native `MERGE` statement.

**Basic configuration**

When you specify only `unique_key`, `dbt-duckdb` uses DuckDB's `UPDATE BY NAME` and `INSERT BY NAME` operations, which automatically match columns by name:

```yml
models:
  - name: my_incremental_model
    config:
      materialized: incremental
      incremental_strategy: merge
      unique_key: id
```

**Enhanced configuration**

Additional options for finer control:

| Configuration                  | Type            | Default | Description                                                   |
| ------------------------------ | --------------- | ------- | ------------------------------------------------------------- |
| `unique_key`                   | `<string/list>` | —       | Required. Columns used for the MERGE join condition.          |
| `incremental_predicates`       | `<list>`        | null    | Additional SQL conditions to filter the MERGE operation.      |
| `merge_update_condition`       | `<string>`      | null    | SQL condition to control when matched records are updated.    |
| `merge_insert_condition`       | `<string>`      | null    | SQL condition to control when unmatched records are inserted. |
| `merge_update_columns`         | `<list>`        | null    | Specific columns to update.                                   |
| `merge_exclude_columns`        | `<list>`        | null    | Columns to exclude from updates.                              |
| `merge_update_set_expressions` | `<dict>`        | null    | Custom expressions for column updates.                        |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

For maximum flexibility, use `merge_clauses` to define custom `when_matched` and `when_not_matched` behaviors. When using DuckLake, MERGE statements are limited to a single UPDATE or DELETE action in `when_matched` clauses due to DuckLake's current MERGE implementation constraints.

In conditions and expressions, use `DBT_INTERNAL_SOURCE` to reference the incoming data and `DBT_INTERNAL_DEST` to reference the existing target table.

### Microbatch strategy[​](#microbatch-strategy "Direct link to Microbatch strategy")

The `microbatch` strategy requires dbt Core 1.9 or later and runs incremental builds in time-based batches using a configured `event_time` column.

| Configuration            | Type       | Default | Description                                                           |
| ------------------------ | ---------- | ------- | --------------------------------------------------------------------- |
| `event_time`             | `<string>` | —       | Required. Name of the timestamp column used for microbatch windowing. |
| `begin`                  | `<string>` | —       | Required. Start time for batching (for example, `2025-01-01`).        |
| `batch_size`             | `<string>` | —       | Required. Batch grain (for example, `day`, `hour`).                   |
| `incremental_predicates` | `<list>`   | null    | Optional additional predicates applied within each batch.             |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

tip

Microbatching might not always be the best option from a performance perspective. DuckDB operates on row groups, not physical partitions (unless you have explicitly partitioned data in a DuckLake). Be sure to test different amounts of threads to match your use case.

<!-- -->

## More information[​](#more-information "Direct link to More information")

* For connection modes and profile setup, refer to [Connect DuckDB](https://docs.getdbt.com/docs/local/connect-data-platform/duckdb-setup.md).
* For adapter source code and plugins, refer to the [`dbt-duckdb` repository](https://github.com/duckdb/dbt-duckdb). For adapter release notes, refer to the [`dbt-duckdb` releases page](https://github.com/duckdb/dbt-duckdb/releases).
* For dbt Core concepts used on this page, refer to [Materializations](https://docs.getdbt.com/docs/build/materializations.md), [Incremental models](https://docs.getdbt.com/docs/build/incremental-models.md), and [Python models](https://docs.getdbt.com/docs/build/python-models.md).
