# Add snapshots to your DAG

## Related documentation[​](#related-documentation "Direct link to Related documentation")

* [Snapshot configurations](https://docs.getdbt.com/reference/snapshot-configs.md)
* [Snapshot properties](https://docs.getdbt.com/reference/snapshot-properties.md)
* [`snapshot` command](https://docs.getdbt.com/reference/commands/snapshot.md)

<!-- -->

Learn by video!

For video tutorials on

<!-- -->

Snapshots

<!-- -->

, go to dbt Learn and check out the [Snapshots](https://learn.getdbt.com/courses/snapshots)

<!-- -->

[ course](https://learn.getdbt.com/courses/snapshots).

## What are snapshots?[​](#what-are-snapshots "Direct link to What are snapshots?")

Analysts often need to "look back in time" at previous data states in their mutable tables. While some source data systems are built in a way that makes accessing historical data possible, this is not always the case. dbt provides a mechanism, **snapshots**, which records changes to a mutable table over time.

Snapshots implement [type-2 Slowly Changing Dimensions](https://en.wikipedia.org/wiki/Slowly_changing_dimension#Type_2:_add_new_row) over mutable source tables. These Slowly Changing Dimensions (or SCDs) identify how a row in a table changes over time. Imagine you have an `orders` table where the `status` field can be overwritten as the order is processed.

| id | status  | updated\_at |
| -- | ------- | ----------- |
| 1  | pending | 2024-01-01  |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Now, imagine that the order goes from "pending" to "shipped". That same record will now look like:

| id | status  | updated\_at |
| -- | ------- | ----------- |
| 1  | shipped | 2024-01-02  |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

This order is now in the "shipped" state, but we've lost the information about when the order was last in the "pending" state. This makes it difficult (or impossible) to analyze how long it took for an order to ship. dbt can "snapshot" these changes to help you understand how values in a row change over time. Here's an example of a snapshot table for the previous example:

| id | status  | updated\_at | dbt\_valid\_from | dbt\_valid\_to |
| -- | ------- | ----------- | ---------------- | -------------- |
| 1  | pending | 2024-01-01  | 2024-01-01       | 2024-01-02     |
| 1  | shipped | 2024-01-02  | 2024-01-02       | `null`         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Configuring snapshots[​](#configuring-snapshots "Direct link to Configuring snapshots")

Configure your snapshots in YAML files to tell dbt how to detect record changes. Define snapshots configurations in YAML files, alongside your models, for a cleaner, faster, and more consistent set up. Place snapshot YAML files in the models directory or in a snapshots directory.

snapshots/orders\_snapshot.yml

```yaml
snapshots:
  - name: string
    relation: relation # source('my_source', 'my_table') or ref('my_model')
    description:  markdown_string
    config:
      database: string
      schema: string
      alias: string
      strategy: timestamp | check
      unique_key: column_name_or_expression
      check_cols: [column_name] | all
      updated_at: column_name
      snapshot_meta_column_names: dictionary
      dbt_valid_to_current: string
      hard_deletes: ignore | invalidate | new_record 
```

The following table outlines the configurations available for snapshots:

| Config                                                                                                            | Description                                                                                                                                                                                                                                                                  | Required?                              | Example                          |
| ----------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------- | -------------------------------- |
| [database](https://docs.getdbt.com/reference/resource-configs/database.md)                                        | Specify a custom database for the snapshot                                                                                                                                                                                                                                   | No                                     | analytics                        |
| [schema](https://docs.getdbt.com/reference/resource-configs/schema.md)                                            | Specify a custom schema for the snapshot                                                                                                                                                                                                                                     | No                                     | snapshots                        |
| [alias](https://docs.getdbt.com/reference/resource-configs/alias.md)                                              | Specify an alias for the snapshot                                                                                                                                                                                                                                            | No                                     | your\_custom\_snapshot           |
| [strategy](https://docs.getdbt.com/reference/resource-configs/strategy.md)                                        | The snapshot strategy to use. Valid values: `timestamp` or `check`                                                                                                                                                                                                           | Yes                                    | timestamp                        |
| [unique\_key](https://docs.getdbt.com/reference/resource-configs/unique_key.md)                                   | A primary key column(s) (string or array) or expression for the record                                                                                                                                                                                                       | Yes                                    | `id` or `[order_id, product_id]` |
| [check\_cols](https://docs.getdbt.com/reference/resource-configs/check_cols.md)                                   | If using the `check` strategy, then the columns to check                                                                                                                                                                                                                     | Only if using the `check` strategy     | \["status"]                      |
| [updated\_at](https://docs.getdbt.com/reference/resource-configs/updated_at.md)                                   | A column in your snapshot query results that indicates when each record was last updated, used in the `timestamp` strategy. May support ISO date strings and unix epoch integers, depending on the data platform you use.                                                    | Only if using the `timestamp` strategy | updated\_at                      |
| [dbt\_valid\_to\_current](https://docs.getdbt.com/reference/resource-configs/dbt_valid_to_current.md)             | Set a custom indicator for the value of `dbt_valid_to` in current snapshot records (like a future date). By default, this value is `NULL`. When configured, dbt will use the specified value instead of `NULL` for `dbt_valid_to` for current records in the snapshot table. | No                                     | string                           |
| [snapshot\_meta\_column\_names](https://docs.getdbt.com/reference/resource-configs/snapshot_meta_column_names.md) | Customize the names of the snapshot meta fields                                                                                                                                                                                                                              | No                                     | dictionary                       |
| [hard\_deletes](https://docs.getdbt.com/reference/resource-configs/hard-deletes.md)                               | Specify how to handle deleted rows from the source. Supported options are `ignore` (default), `invalidate` (replaces the legacy `invalidate_hard_deletes=true`), and `new_record`.                                                                                           | No                                     | string                           |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

* In v1.9, `target_schema` became optional, allowing snapshots to be environment-aware. By default, without `target_schema` or `target_database` defined, snapshots now use the `generate_schema_name` or `generate_database_name` macros to determine where to build.
* Developers can still set a custom location with [`schema`](https://docs.getdbt.com/reference/resource-configs/schema.md) and [`database`](https://docs.getdbt.com/reference/resource-configs/database.md) configs, consistent with other resource types.
* A number of other configurations are also supported (for example, `tags` and `post-hook`). For the complete list, refer to [Snapshot configurations](https://docs.getdbt.com/reference/snapshot-configs.md).
* You can configure snapshots from both the `dbt_project.yml` file and a `config` block. For more information, refer to the [configuration docs](https://docs.getdbt.com/reference/snapshot-configs.md).
* Starting dbt Core v1.12, you can inspect the SQL generated for snapshots by running [`dbt compile`](https://docs.getdbt.com/reference/commands/compile.md). You can find compiled SQL files in the `target/compiled/` directory of your dbt project.

### Add a snapshot to your project[​](#add-a-snapshot-to-your-project "Direct link to Add a snapshot to your project")

To add a snapshot to your project follow these steps. For users on versions 1.8 and earlier, refer to [Legacy snapshot configurations](https://docs.getdbt.com/reference/resource-configs/snapshots-jinja-legacy.md).

1. Create a properties YAML file in your `snapshots` directory: `snapshots/orders_snapshot.yml` and add your configuration details. You can also configure your snapshot from your project YAML file (`dbt_project.yml`) ([docs](https://docs.getdbt.com/reference/snapshot-configs.md)).

   snapshots/orders\_snapshot.yml

   ```yaml
   snapshots:
     - name: orders_snapshot
       relation: source('jaffle_shop', 'orders')
       config:
         schema: snapshots
         database: analytics
         unique_key: id
         strategy: timestamp
         updated_at: updated_at
         dbt_valid_to_current: "to_date('9999-12-31')" # Specifies that current records should have `dbt_valid_to` set to `'9999-12-31'` instead of `NULL`.
   ```

2. (Optional) Apply transformations using an ephemeral model. By default, snapshots reference a source directly (as shown in the YAML in the previous step). If you need to apply transformations (such as filtering or deduplication), define an ephemeral model first to apply those transformations, and reference it in the snapshot relation field instead of calling `source()` directly.

   For example, here's an ephemeral model:

   models/ephemeral\_orders.sql

   ```sql
   {{ config(materialized='ephemeral') }}

   select * from {{ source('jaffle_shop', 'orders') }}
   ```

   This is how to reference the ephemeral model in the `relation` field:

   snapshots/orders\_snapshot.yml

   ```yaml
     snapshots:
       - name: orders_snapshot
         relation: ref('ephemeral_orders')
         ... rest of config...
   ```

3. Check whether the result set of your query includes a reliable timestamp column that indicates when a record was last updated. For our example, the `updated_at` column reliably indicates record changes, so we can use the `timestamp` strategy. If your query result set does not have a reliable timestamp, you'll need to instead use the `check` strategy — more details on this below.

4. Run the `dbt snapshot` [command](https://docs.getdbt.com/reference/commands/snapshot.md) — for our example, a new table will be created at `analytics.snapshots.orders_snapshot`. The [`schema`](https://docs.getdbt.com/reference/resource-configs/schema.md) config will utilize the `generate_schema_name` macro.

   ```text
   $ dbt snapshot
   Running with dbt=1.9.0

   15:07:36 | Concurrency: 8 threads (target='dev')
   15:07:36 |
   15:07:36 | 1 of 1 START snapshot snapshots.orders_snapshot...... [RUN]
   15:07:36 | 1 of 1 OK snapshot snapshots.orders_snapshot..........[SELECT 3 in 1.82s]
   15:07:36 |
   15:07:36 | Finished running 1 snapshots in 0.68s.

   Completed successfully

   Done. PASS=2 ERROR=0 SKIP=0 TOTAL=1
   ```

   Compiled SQL for snapshots[Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

   Starting dbt Core v1.12, you can inspect the SQL generated for this snapshot by running [`dbt compile`](https://docs.getdbt.com/reference/commands/compile.md) or `dbt compile --select orders_snapshot`.

   Open the compiled SQL in `target/compiled/` to inspect or debug the generated queries. Each snapshot is compiled into its own SQL file, even if multiple snapshots are defined in the same source file.

5. Inspect the results by selecting from the table dbt created (`analytics.snapshots.orders_snapshot`). After the first run, you should see the results of your query, plus the [snapshot meta fields](#snapshot-meta-fields) as described later on.

6. Run the `dbt snapshot` command again and inspect the results. If any records have been updated, the snapshot should reflect this.

7. Select from the `snapshot` in downstream models using the `ref` function.

   models/changed\_orders.sql

   ```sql
   select * from {{ ref('orders_snapshot') }}
   ```

8. Snapshots are only useful if you run them frequently — schedule the `dbt snapshot` command to run regularly.

### Configuration best practices[​](#configuration-best-practices "Direct link to Configuration best practices")

 Use the timestamp strategy where possible

The timestamp strategy is recommended because it handles column additions and deletions more efficiently than the `check` strategy. This is because it's more robust to schema changes, especially when columns are added or removed over time.

The timestamp strategy relies on a single `updated_at` field, which means it avoids the need to constantly update your snapshot configuration as your source table evolves.

Why timestamp is the preferred strategy:

* Requires tracking only one column (`updated_at`)
* Automatically handles new or removed columns in the source table
* Less prone to errors when the table schema evolves over time (for example, if using the `check` strategy, you might need to update the `check_cols` configuration)

 Use dbt\_valid\_to\_current for easier date range queries

By default, `dbt_valid_to` is `NULL` for current records. However, if you set the [`dbt_valid_to_current` configuration](https://docs.getdbt.com/reference/resource-configs/dbt_valid_to_current.md) (available in dbt Core v1.9+), `dbt_valid_to` will be set to your specified value (such as `9999-12-31`) for current records.

This allows for straightforward date range filtering.

 Ensure your unique key is really unique

The unique key is used by dbt to match rows up, so it's extremely important to make sure this key is actually unique! If you're snapshotting a source, I'd recommend adding a uniqueness test to your source ([example](https://github.com/dbt-labs/jaffle_shop/blob/8e7c853c858018180bef1756ec93e193d9958c5b/models/staging/schema.yml#L26)).

 Use a schema that is separate to your models' schema

Snapshots can't be rebuilt. Because of this, it's a good idea to put snapshots in a separate schema so end users know they're special. From there, you may want to set different privileges on your snapshots compared to your models, and even run them as a different user (or role, depending on your warehouse) to make it very difficult to drop a snapshot unless you really want to.

 Use ephemeral model to clean or transform data before snapshotting

If you need to clean or transform your data before snapshotting, create an ephemeral model or a staging model that applies the necessary transformations. Then, reference this model in your snapshot configuration. This approach keeps your snapshot definitions clean and allows you to test and run transformations separately.

### How snapshots work[​](#how-snapshots-work "Direct link to How snapshots work")

When you run the [`dbt snapshot` command](https://docs.getdbt.com/reference/commands/snapshot.md):

* **On the first run:** dbt will create the initial snapshot table — this will be the result set of your `select` statement, with additional columns including `dbt_valid_from` and `dbt_valid_to`. All records will have a `dbt_valid_to = null` or the value specified in [`dbt_valid_to_current`](https://docs.getdbt.com/reference/resource-configs/dbt_valid_to_current.md) (available in dbt Core 1.9+) if configured.

* **On subsequent runs:** dbt will check which records have changed or if any new records have been created:

  <!-- -->

  * The `dbt_valid_to` column will be updated for any existing records that have changed.
  * The updated record and any new records will be inserted into the snapshot table. These records will now have `dbt_valid_to = null` or the value configured in `dbt_valid_to_current` (available in dbt Core v1.9+).

#### Note[​](#note "Direct link to Note")

* These column names can be customized to your team or organizational conventions using the [snapshot\_meta\_column\_names](#snapshot-meta-fields) config.
* Use the `dbt_valid_to_current` config to set a custom indicator for the value of `dbt_valid_to` in current snapshot records (like a future date such as `9999-12-31`). By default, this value is `NULL`. When set, dbt will use this specified value instead of `NULL` for `dbt_valid_to` for current records in the snapshot table.
* Use the [`hard_deletes`](https://docs.getdbt.com/reference/resource-configs/hard-deletes.md) config to track hard deletes by adding a new record when row become "deleted" in source. Supported options are `ignore`, `invalidate`, and `new_record`.

Snapshots can be referenced in downstream models the same way as referencing models — by using the [ref](https://docs.getdbt.com/reference/dbt-jinja-functions/ref.md) function.

## Detecting row changes[​](#detecting-row-changes "Direct link to Detecting row changes")

Snapshot "strategies" define how dbt knows if a row has changed. There are two strategies built-in to dbt:

* [Timestamp](#timestamp-strategy-recommended) — Uses an `updated_at` column to determine if a row has changed.
* [Check](#check-strategy) — Compares a list of columns between their current and historical values to determine if a row has changed.

### Timestamp strategy (recommended)[​](#timestamp-strategy-recommended "Direct link to Timestamp strategy (recommended)")

The `timestamp` strategy uses an `updated_at` field to determine if a row has changed. If the configured `updated_at` column for a row is more recent than the last time the snapshot ran, then dbt will invalidate the old record and record the new one. If the timestamps are unchanged, then dbt will not take any action.

Why timestamp is recommended?

* Requires tracking only one column (`updated_at`)
* Automatically handles new or removed columns in the source table
* Less prone to errors when the table schema evolves over time (for example, if using the `check` strategy, you might need to update the `check_cols` configuration)

The `timestamp` strategy requires the following configurations:

| Config      | Description                                                                                                                                                   | Example      |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ |
| updated\_at | A column which represents when the source row was last updated. May support ISO date strings and unix epoch integers, depending on the data platform you use. | `updated_at` |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

**Example usage:**

snapshots/orders\_snapshot.yml

```yaml
snapshots:
  - name: orders_snapshot_timestamp
    relation: source('jaffle_shop', 'orders')
    config:
      schema: snapshots
      unique_key: id
      strategy: timestamp
      updated_at: updated_at
```

### Check strategy[​](#check-strategy "Direct link to Check strategy")

The `check` strategy is useful for tables which do not have a reliable `updated_at` column. This strategy works by comparing a list of columns between their current and historical values. If any of these columns have changed, then dbt will invalidate the old record and record the new one. If the column values are identical, then dbt will not take any action.

The `check` strategy requires the following configurations:

| Config      | Description                                                           | Example             |
| ----------- | --------------------------------------------------------------------- | ------------------- |
| check\_cols | A list of columns to check for changes, or `all` to check all columns | `["name", "email"]` |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

check\_cols = 'all'

The `check` snapshot strategy can be configured to track changes to *all* columns by supplying `check_cols = 'all'`. It is better to explicitly enumerate the columns that you want to check. Consider using a surrogate key to condense many columns into a single column.

#### Example usage[​](#example-usage "Direct link to Example usage")

snapshots/orders\_snapshot.yml

```yaml
snapshots:
  - name: orders_snapshot_check
    relation: source('jaffle_shop', 'orders')
    config:
      schema: snapshots
      unique_key: id
      strategy: check
      check_cols:
        - status
        - is_cancelled
```

#### Example usage with `updated_at`[​](#example-usage-with-updated_at "Direct link to example-usage-with-updated_at")

When using the `check` strategy, dbt tracks changes by comparing values in `check_cols`. By default, dbt uses the timestamp to update `dbt_updated_at`, `dbt_valid_from` and `dbt_valid_to` fields. Optionally you can set an `updated_at` column:

* If `updated_at` is configured, the `check` strategy uses this column instead, as with the timestamp strategy.
* If `updated_at` value is null, dbt defaults to using the current timestamp.

Check out the following example, which shows how to use the `check` strategy with `updated_at`:

```yaml
snapshots:
  - name: orders_snapshot
    relation: ref('stg_orders')
    config:
      schema: snapshots
      unique_key: order_id
      strategy: check
      check_cols:
        - status
        - is_cancelled
      updated_at: updated_at
```

In this example:

* If at least one of the specified `check_cols `changes, the snapshot creates a new row. If the `updated_at` column has a value (is not null), the snapshot uses it; otherwise, it defaults to the timestamp.
* If `updated_at` isn’t set, then dbt automatically falls back to [using the current timestamp](#sample-results-for-the-check-strategy) to track changes.
* Use this approach when your `updated_at` column isn't reliable for tracking record updates, but you still want to use it — rather than the snapshot's execution time — whenever row changes are detected.

### Hard deletes (opt-in)[​](#hard-deletes-opt-in "Direct link to Hard deletes (opt-in)")

In dbt v1.9 and higher, the [`hard_deletes`](https://docs.getdbt.com/reference/resource-configs/hard-deletes.md) config replaces the `invalidate_hard_deletes` config to give you more control on how to handle deleted rows from the source. The `hard_deletes` config is not a separate strategy but an additional opt-in feature that can be used with any snapshot strategy.

The `hard_deletes` config has three options/fields:

| Field              | Description                                                                                                                       |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------- |
| `ignore` (default) | No action for deleted records.                                                                                                    |
| `invalidate`       | Behaves the same as the existing `invalidate_hard_deletes=true`, where deleted records are invalidated by setting `dbt_valid_to`. |
| `new_record`       | Tracks deleted records as new rows using the `dbt_is_deleted` [meta field](#snapshot-meta-fields) when records are deleted.       |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

 When to use the hard\_deletes and invalidate\_hard\_deletes config?

**Use `invalidate_hard_deletes` (v1.8 and earlier) if:**

* Gaps in the snapshot history (missing records for deleted rows) are acceptable.
* You want to invalidate deleted rows by setting their `dbt_valid_to` timestamp to the current time (implicit delete).
* You are working with smaller datasets where tracking deletions as a separate state is unnecessary.

**Use `hard_deletes: new_record` (v1.9 and higher) if:**

* You want to maintain continuous snapshot history without gaps.
* You want to explicitly track deletions by adding new rows with a `dbt_is_deleted` column (explicit delete).
* You are working with larger datasets where explicitly tracking deleted records improves data lineage clarity.

#### Example usage[​](#example-usage-1 "Direct link to Example usage")

snapshots/orders\_snapshot.yml

```yaml
snapshots:
  - name: orders_snapshot_hard_delete
    relation: source('jaffle_shop', 'orders')
    config:
      schema: snapshots
      unique_key: id
      strategy: timestamp
      updated_at: updated_at
      hard_deletes: new_record  # options are: 'ignore', 'invalidate', or 'new_record'
```

In this example, the `hard_deletes: new_record` config will add a new row for deleted records with the `dbt_is_deleted` column set to `True`. Any restored records are added as new rows with the `dbt_is_deleted` field set to `False`.

The resulting table will look like this:

| id | status   | updated\_at      | dbt\_valid\_from | dbt\_valid\_to   | dbt\_is\_deleted |
| -- | -------- | ---------------- | ---------------- | ---------------- | ---------------- |
| 1  | pending  | 2024-01-01 10:47 | 2024-01-01 10:47 | 2024-01-01 11:05 | False            |
| 1  | shipped  | 2024-01-01 11:05 | 2024-01-01 11:05 | 2024-01-01 11:20 | False            |
| 1  | deleted  | 2024-01-01 11:20 | 2024-01-01 11:20 | 2024-01-01 12:00 | True             |
| 1  | restored | 2024-01-01 12:00 | 2024-01-01 12:00 |                  | False            |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Snapshot meta-fields[​](#snapshot-meta-fields "Direct link to Snapshot meta-fields")

Snapshot tables will be created as a clone of your source dataset, plus some additional meta-fields\*.

In dbt Core v1.9+ (or available sooner in [the **Latest** release track in dbt](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md)):

* These column names can be customized to your team or organizational conventions using the [`snapshot_meta_column_names`](https://docs.getdbt.com/reference/resource-configs/snapshot_meta_column_names.md) config.
* Use the [`dbt_valid_to_current` config](https://docs.getdbt.com/reference/resource-configs/dbt_valid_to_current.md) to set a custom indicator for the value of `dbt_valid_to` in current snapshot records (like a future date such as `9999-12-31`). By default, this value is `NULL`. When set, dbt will use this specified value instead of `NULL` for `dbt_valid_to` for current records in the snapshot table.
* Use the [`hard_deletes`](https://docs.getdbt.com/reference/resource-configs/hard-deletes.md) config to track deleted records as new rows with the `dbt_is_deleted` meta field when using the `hard_deletes='new_record'` field.

| Field            | Meaning                                                                                                                                          | Notes                                                                                          | Example                                                       |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------- | ------------------------------------------------------------- |
| `dbt_valid_from` | The timestamp when this snapshot row was first inserted and became valid.                                                                        | This column can be used to order the different "versions" of a record.                         | `snapshot_meta_column_names: {dbt_valid_from: start_date}`    |
| `dbt_valid_to`   | The timestamp when this row became invalidated. For current records, this is `NULL` by default or the value specified in `dbt_valid_to_current`. | The most recent snapshot record will have `dbt_valid_to` set to `NULL` or the specified value. | `snapshot_meta_column_names: {dbt_valid_to: end_date}`        |
| `dbt_scd_id`     | A unique key generated for each snapshot row.                                                                                                    | This is used internally by dbt.                                                                | `snapshot_meta_column_names: {dbt_scd_id: scd_id}`            |
| `dbt_updated_at` | The `updated_at` timestamp of the source record when this snapshot row was inserted.                                                             | This is used internally by dbt.                                                                | `snapshot_meta_column_names: {dbt_updated_at: modified_date}` |
| `dbt_is_deleted` | A string value indicating if the record has been deleted. (`True` if deleted, `False` if not deleted).                                           | Added when `hard_deletes='new_record'` is configured.                                          | `snapshot_meta_column_names: {dbt_is_deleted: is_deleted}`    |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

All of these column names can be customized using the `snapshot_meta_column_names` config. Refer to this [example](https://docs.getdbt.com/reference/resource-configs/snapshot_meta_column_names.md#example) for more details.

\*The timestamps used for each column are subtly different depending on the strategy you use:

* For the `timestamp` strategy, the configured `updated_at` column is used to populate the `dbt_valid_from`, `dbt_valid_to` and `dbt_updated_at` columns.

   Sample results for the timestamp strategy

  Snapshot query results at `2024-01-01 11:00`

  | id | status  | updated\_at      |
  | -- | ------- | ---------------- |
  | 1  | pending | 2024-01-01 10:47 |

  Search table...

  |                  |   |   |   |   |
  | ---------------- | - | - | - | - |
  | Loading table... |   |   |   |   |

  Snapshot results (note that `11:00` is not used anywhere):

  | id | status  | updated\_at      | dbt\_valid\_from | dbt\_valid\_to | dbt\_updated\_at |
  | -- | ------- | ---------------- | ---------------- | -------------- | ---------------- |
  | 1  | pending | 2024-01-01 10:47 | 2024-01-01 10:47 |                | 2024-01-01 10:47 |

  Search table...

  |                  |   |   |   |   |
  | ---------------- | - | - | - | - |
  | Loading table... |   |   |   |   |

  Query results at `2024-01-01 11:30`:

  | id | status  | updated\_at      |
  | -- | ------- | ---------------- |
  | 1  | shipped | 2024-01-01 11:05 |

  Search table...

  |                  |   |   |   |   |
  | ---------------- | - | - | - | - |
  | Loading table... |   |   |   |   |

  Snapshot results (note that `11:30` is not used anywhere):

  | id | status  | updated\_at      | dbt\_valid\_from | dbt\_valid\_to   | dbt\_updated\_at |
  | -- | ------- | ---------------- | ---------------- | ---------------- | ---------------- |
  | 1  | pending | 2024-01-01 10:47 | 2024-01-01 10:47 | 2024-01-01 11:05 | 2024-01-01 10:47 |
  | 1  | shipped | 2024-01-01 11:05 | 2024-01-01 11:05 |                  | 2024-01-01 11:05 |

  Search table...

  |                  |   |   |   |   |
  | ---------------- | - | - | - | - |
  | Loading table... |   |   |   |   |

  Snapshot results with `hard_deletes='new_record'`:

  | id | status  | updated\_at      | dbt\_valid\_from | dbt\_valid\_to   | dbt\_updated\_at | dbt\_is\_deleted |
  | -- | ------- | ---------------- | ---------------- | ---------------- | ---------------- | ---------------- |
  | 1  | pending | 2024-01-01 10:47 | 2024-01-01 10:47 | 2024-01-01 11:05 | 2024-01-01 10:47 | False            |
  | 1  | shipped | 2024-01-01 11:05 | 2024-01-01 11:05 | 2024-01-01 11:20 | 2024-01-01 11:05 | False            |
  | 1  | deleted | 2024-01-01 11:20 | 2024-01-01 11:20 |                  | 2024-01-01 11:20 | True             |

  Search table...

  |                  |   |   |   |   |
  | ---------------- | - | - | - | - |
  | Loading table... |   |   |   |   |

* For the `check` strategy, the current timestamp is used to populate each column. If configured, the `check` strategy uses the `updated_at` column instead, as with the timestamp strategy.

   Sample results for the check strategy

  Snapshot query results at `2024-01-01 11:00`

  | id | status  |
  | -- | ------- |
  | 1  | pending |

  Search table...

  |                  |   |   |   |   |
  | ---------------- | - | - | - | - |
  | Loading table... |   |   |   |   |

  Snapshot results:

  | id | status  | dbt\_valid\_from | dbt\_valid\_to | dbt\_updated\_at |
  | -- | ------- | ---------------- | -------------- | ---------------- |
  | 1  | pending | 2024-01-01 11:00 |                | 2024-01-01 11:00 |

  Search table...

  |                  |   |   |   |   |
  | ---------------- | - | - | - | - |
  | Loading table... |   |   |   |   |

  Query results at `2024-01-01 11:30`:

  | id | status  |
  | -- | ------- |
  | 1  | shipped |

  Search table...

  |                  |   |   |   |   |
  | ---------------- | - | - | - | - |
  | Loading table... |   |   |   |   |

  Snapshot results:

  | id | status  | dbt\_valid\_from | dbt\_valid\_to   | dbt\_updated\_at |
  | -- | ------- | ---------------- | ---------------- | ---------------- |
  | 1  | pending | 2024-01-01 11:00 | 2024-01-01 11:30 | 2024-01-01 11:00 |
  | 1  | shipped | 2024-01-01 11:30 |                  | 2024-01-01 11:30 |

  Search table...

  |                  |   |   |   |   |
  | ---------------- | - | - | - | - |
  | Loading table... |   |   |   |   |

  Snapshot results with `hard_deletes='new_record'`:

  | id | status  | dbt\_valid\_from | dbt\_valid\_to   | dbt\_updated\_at | dbt\_is\_deleted |
  | -- | ------- | ---------------- | ---------------- | ---------------- | ---------------- |
  | 1  | pending | 2024-01-01 11:00 | 2024-01-01 11:30 | 2024-01-01 11:00 | False            |
  | 1  | shipped | 2024-01-01 11:30 | 2024-01-01 11:40 | 2024-01-01 11:30 | False            |
  | 1  | deleted | 2024-01-01 11:40 |                  | 2024-01-01 11:40 | True             |

  Search table...

  |                  |   |   |   |   |
  | ---------------- | - | - | - | - |
  | Loading table... |   |   |   |   |

## FAQs[​](#faqs "Direct link to FAQs")

How do I run one snapshot at a time?

To run one snapshot, use the `--select` flag, followed by the name of the snapshot:

```shell
$ dbt snapshot --select order_snapshot
```

Check out the [model selection syntax documentation](https://docs.getdbt.com/reference/node-selection/syntax.md) for more operators and examples.

How often should I run the snapshot command?

Snapshots are a batch-based approach to [change data capture](https://en.wikipedia.org/wiki/Change_data_capture). The `dbt snapshot` command must be run on a schedule to ensure that changes to tables are actually recorded! While individual use-cases may vary, snapshots are intended to be run between hourly and daily. If you find yourself snapshotting more frequently than that, consider if there isn't a more appropriate way to capture changes in your source data tables.

What happens if I add new columns to my snapshot query?

When the columns of your source query changes, dbt will attempt to reconcile this change in the destination snapshot table. dbt does this by:

1. Creating new columns from the source query in the destination table
2. Expanding the size of string types where necessary (eg. `varchar`s on Redshift)

dbt *will not* delete columns in the destination snapshot table if they are removed from the source query. It will also not change the type of a column beyond expanding the size of varchar columns. That is, if a `string` column is changed to a `date` column in the snapshot source query, dbt will not attempt to change the type of the column in the destination table.

Do hooks run with snapshots?

Yes! The following hooks are available for snapshots:

* [pre-hooks](https://docs.getdbt.com/reference/resource-configs/pre-hook-post-hook.md)
* [post-hooks](https://docs.getdbt.com/reference/resource-configs/pre-hook-post-hook.md)
* [on-run-start](https://docs.getdbt.com/reference/project-configs/on-run-start-on-run-end.md)
* [on-run-end](https://docs.getdbt.com/reference/project-configs/on-run-start-on-run-end.md)

Can I store my snapshots in a directory other than the \`snapshot\` directory in my project?

By default, dbt expects your snapshot files to be located in the `snapshots` subdirectory of your project.

To change this, update the [snapshot-paths](https://docs.getdbt.com/reference/project-configs/snapshot-paths.md) configuration in your `dbt_project.yml` file, like so:

dbt\_project.yml

```yml
snapshot-paths: ["snapshots"]
```

Note that you cannot co-locate snapshots and models in the same directory.

Debug Snapshot target is not a snapshot table errors

If you see the following error when you try executing the snapshot command:

> Snapshot target is not a snapshot table (missing `dbt_scd_id`, `dbt_valid_from`, `dbt_valid_to`)

Double check that you haven't inadvertently caused your snapshot to behave like table materializations by setting its `materialized` config to be `table`. Prior to dbt version 1.4, it was possible to have a snapshot like this:

```sql
{% snapshot snappy %}
  {{ config(materialized = 'table', ...) }}
  ...
{% endsnapshot %}
```

dbt is treating snapshots like tables (issuing `create or replace table ...` statements) **silently** instead of actually snapshotting data (SCD2 via `insert` / `merge` statements). When upgrading to dbt versions 1.4 and higher, dbt now raises a Parsing Error (instead of silently treating snapshots like tables) that reads:

```text
A snapshot must have a materialized value of 'snapshot'
```

This tells you to change your `materialized` config to `snapshot`. But when you make that change, you might encounter an error message saying that certain fields like `dbt_scd_id` are missing. This error happens because, previously, when dbt treated snapshots as tables, it didn't include the necessary [snapshot meta-fields](https://docs.getdbt.com/docs/build/snapshots.md#snapshot-meta-fields) in your target table. Since those meta-fields don't exist, dbt correctly identifies that you're trying to create a snapshot in a table that isn't actually a snapshot.

When this happens, you have to start from scratch — re-snapshotting your source data as if it was the first time by dropping your "snapshot" which isn't a real snapshot table. Then dbt snapshot will create a new snapshot and insert the snapshot meta-fields as expected.
