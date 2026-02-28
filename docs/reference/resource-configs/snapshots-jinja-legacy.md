# Legacy snapshot configuration legacy

Use legacy SQL-based snapshot configurations with Jinja blocks in any dbt version. dbt v1.9 introduced YAML-based configs for better readability and environment awareness.

There are situations where you want to use the legacy syntax for [snapshots](https://docs.getdbt.com/docs/build/snapshots.md) in any dbt version or release track. This page details how you can use the legacy SQL-based configurations if you need to.

In dbt v1.9, this syntax was replaced with a [YAML-based configuration](https://docs.getdbt.com/reference/snapshot-configs.md#configuring-snapshots) in [dbt's **Latest** release track](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md). The benefits of YAML-based configurations are that the snapshots are environment aware, meaning you don't have to specify `schema` or `database`, and the syntax is more concise.

For new snapshots, we recommend using these latest YAML-based configs. If you'd like to move to the YAML-based configuration for existing snapshots, you can [migrate over](https://docs.getdbt.com/reference/snapshot-configs.md#snapshot-configuration-migration).

When would you want to use the SQL-based syntax and YAML-based syntax?

* SQL-based syntax:

  * Defined in `.sql` files within a snapshot Jinja block, typically located in your `snapshots` directory. Available in all versions.
  * Useful for existing snapshots already using this syntax.
  * Suitable for performing very light transformations (but creating a separate ephemeral model for transformations is recommended for better maintainability).

* YAML-based syntax:

  * Defined in `whatever_name.yml` or in the `snapshots` or `models` directory you prefer. Available in dbt's **Latest** release track and dbt v1.9 and later.
  * Ideal for new snapshots or existing snapshots that need to be [migrated](https://docs.getdbt.com/reference/snapshot-configs.md#snapshot-configuration-migration).
  * Create transformations separate from the snapshot file by creating an ephemeral model and referencing it in the snapshot using the `relation` field.

## Snapshot configurations[​](#snapshot-configurations "Direct link to Snapshot configurations")

Although you can use the more performant YAML-based configuration, you might still want to use the legacy configuration to define your snapshots if it suits your needs.

Snapshots can be configured in two main ways:

* Using [snapshot-specific configurations](#snapshot-specific-configurations)
* Or using [general configurations](#general-configuration)

These configurations allow you to control how dbt detects changes in your data and where snapshots are stored. Both types of configurations can coexist in your project in the same `config` block (or from your `dbt_project.yml` file or `properties.yaml` file).

One of the most important configs you can decide is [strategies](#snapshot-strategies), which tells dbt how to detect modified rows.

### Snapshot specific configurations[​](#snapshot-specific-configurations "Direct link to Snapshot specific configurations")

Snapshot-specific configurations are applicable to only one dbt resource type rather than multiple resource types. You can define these settings within the resource’s file using the `{{ config() }}` macro (as well as in the project file (`dbt_project.yml`) or a property file (`models/properties.yml` for models, similarly for other resources)).

snapshots/orders\_snapshot.sql

```sql
{ % snapshot orders_snapshot %}

{{ config(
    target_schema="<string>",
    target_database="<string>",
    unique_key="<column_name_or_expression>",
    strategy="timestamp" | "check",
    updated_at="<column_name>",
    check_cols=["<column_name>"] | "all"
    invalidate_hard_deletes=true | false
) 
}}

select * from {{ source('jaffle_shop', 'orders') }}

{% endsnapshot %}
```

### General configuration[​](#general-configuration "Direct link to General configuration")

Use general configurations for broader operational settings applicable across multiple resource types. Like resource-specific configurations, these can also be set in the project YAML file, properties YAML files, or within resource-specific files using a config block.

snapshots/snapshot.sql

```sql
{{ config(
    enabled=true | false,
    tags="<string>" | ["<string>"],
    alias="<string>", 
    pre_hook="<sql-statement>" | ["<sql-statement>"],
    post_hook="<sql-statement>" | ["<sql-statement>"]
    persist_docs={<dict>}
    grants={<dict>}
) }}
```

### Snapshot strategies[​](#snapshot-strategies "Direct link to Snapshot strategies")

Snapshot "strategies" define how dbt knows if a row has changed. There are two strategies built-in to dbt that require the `strategy` parameter:

* [Timestamp](https://docs.getdbt.com/reference/resource-configs/snapshots-jinja-legacy.md?strategy=timestamp#snapshot-strategies) — Uses an `updated_at` column to determine if a row has changed.
* [Check](https://docs.getdbt.com/reference/resource-configs/snapshots-jinja-legacy.md?strategy=check#snapshot-strategies) — Compares a list of columns between their current and historical values to determine if a row has changed. Uses the `check_cols` parameter.

- Timestamp
- Check

The timestamp strategy uses an `updated_at` field to determine if a row has changed. If the configured `updated_at` column for a row is more recent than the last time the snapshot ran, then dbt will invalidate the old record and record the new one. If the timestamps are unchanged, then dbt will not take any action.

#### Example[​](#example "Direct link to Example")

snapshots/timestamp\_example.sql

```sql
{% snapshot orders_snapshot_timestamp %}

    {{
        config(
          target_schema='snapshots',
          strategy='timestamp',
          unique_key='id',
          updated_at='updated_at',
        )
    }}

    select * from {{ source('jaffle_shop', 'orders') }}

{% endsnapshot %}
```

The check strategy is useful for tables which do not have a reliable `updated_at` column. It requires the `check_cols` parameter, which is a list of columns within the results of your snapshot query to check for changes. Alternatively, use all columns using the all value (however this may be less performant).

#### Example[​](#example-1 "Direct link to Example")

snapshots/check\_example.sql

```sql
{% snapshot orders_snapshot_check %}

    {{
        config(
          strategy='check',
          unique_key='id',
          check_cols=['status', 'is_cancelled'],
        )
    }}

    select * from {{ source('jaffle_shop', 'orders') }}

{% endsnapshot %}
```

#### Examples[​](#examples "Direct link to Examples")

 Check a list of columns for changes

snapshots/check\_example.sql

```sql
{% snapshot orders_snapshot_check %}

    {{
        config(
          strategy='check',
          unique_key='id',
          check_cols=['status', 'is_cancelled'],
        )
    }}

    select * from {{ source('jaffle_shop', 'orders') }}

{% endsnapshot %}
```

 Check all columns for changes

snapshots/check\_example.sql

```sql
{% snapshot orders_snapshot_check %}

    {{
        config(
          strategy='check',
          unique_key='id',
          check_cols='all',
        )
    }}

    select * from {{ source('jaffle_shop', 'orders') }}

{% endsnapshot %}
```

## Configuration reference[​](#configuration-reference "Direct link to Configuration reference")

Configure your snapshot to tell dbt how to detect record changes. Snapshots are `select` statements, defined within a snapshot block in a `.sql` file (typically in your `snapshots` directory or any other directory).

The following table outlines the configurations available for snapshots:

<!-- -->

## Add snapshot to a project[​](#add-snapshot-to-a-project "Direct link to Add snapshot to a project")

To add a snapshot to your project:

1. Create a file in your `snapshots` directory with a `.sql` file extension. For example,`snapshots/orders.sql`
2. Use a `snapshot` block to define the start and end of a snapshot:

snapshots/orders\_snapshot.sql

```sql
{% snapshot orders_snapshot %}

{% endsnapshot %}
```

3. Write a `select` statement within the snapshot block (tips for writing a good snapshot query are below). This select statement defines the results that you want to snapshot over time. You can use `sources` or `refs` here.

snapshots/orders\_snapshot.sql

```sql
{% snapshot orders_snapshot %}

select * from {{ source('jaffle_shop', 'orders') }}

{% endsnapshot %}
```

4. Check whether the result set of your query includes a reliable timestamp column that indicates when a record was last updated. For our example, the `updated_at` column reliably indicates record changes, so we can use the `timestamp` strategy. If your query result set does not have a reliable timestamp, you'll need to instead use the `check` strategy — more details on this in the next step.

5. Add configurations to your snapshot using a `config` block. You can also [configure your snapshot from your `dbt_project.yml` file](https://docs.getdbt.com/reference/snapshot-configs.md).

<!-- -->

6. Run the `dbt snapshot` [command](https://docs.getdbt.com/reference/commands/snapshot.md). For our example, a new table will be created at `analytics.snapshots.orders_snapshot`. You can change the `target_database` configuration, the `target_schema` configuration and the name of the snapshot (as defined in `{% snapshot .. %}`) will change how dbt names this table.

```dbt
Running with dbt=1.8.0

15:07:36 | Concurrency: 8 threads (target='dev')
15:07:36 |
15:07:36 | 1 of 1 START snapshot snapshots.orders_snapshot...... [RUN]
15:07:36 | 1 of 1 OK snapshot snapshots.orders_snapshot..........[SELECT 3 in 1.82s]
15:07:36 |
15:07:36 | Finished running 1 snapshots in 0.68s.

Completed successfully

Done. PASS=2 ERROR=0 SKIP=0 TOTAL=1
```

1. Inspect the results by selecting from the table dbt created. After the first run, you should see the results of your query, plus the [snapshot meta fields](https://docs.getdbt.com/docs/build/snapshots.md#snapshot-meta-fields) as described earlier.

2. Run the `dbt snapshot` command again, and inspect the results. If any records have been updated, the snapshot should reflect this.

3. Select from the `snapshot` in downstream models using the `ref` function.

models/changed\_orders.sql

```sql
select * from {{ ref('orders_snapshot') }}
```

10. Snapshots are only useful if you run them frequently — schedule the `snapshot` command to run regularly.

## Examples[​](#examples-1 "Direct link to Examples")

This section outlines some examples of how to apply configurations to snapshots using the legacy method.

 Apply configurations to one snapshot only

Use config blocks if you need to apply a configuration to one snapshot only.

snapshots/postgres\_app/orders\_snapshot.sql

```sql
{% snapshot orders_snapshot %}
    {{
        config(
          unique_key='id',
          strategy='timestamp',
          updated_at='updated_at'
        )
    }}
    -- Pro-Tip: Use sources in snapshots!
    select * from {{ source('jaffle_shop', 'orders') }}
{% endsnapshot %}
```

 Using the updated\_at parameter

The `updated_at` parameter is required if using the timestamp strategy. The `updated_at` parameter is a column within the results of your snapshot query that represents when the record row was last updated.

snapshots/orders.sql

```sql
{{ config(
  strategy="timestamp",
  updated_at="column_name"
) }}
```

#### Examples[​](#examples-2 "Direct link to Examples")

* #### Using a column name `updated_at`:[​](#using-a-column-name-updated_at "Direct link to using-a-column-name-updated_at")
  <!-- -->

* #### Coalescing two columns to create a reliable `updated_at` column:[​](#coalescing-two-columns-to-create-a-reliable-updated_at-column "Direct link to coalescing-two-columns-to-create-a-reliable-updated_at-column")

  Consider a data source that only has an `updated_at` column filled in when a record is updated (so a `null` value indicates that the record hasn't been updated after it was created).

  Since the `updated_at` configuration only takes a column name, rather than an expression, you should update your snapshot query to include the coalesced column.

  <!-- -->

 Using the unique\_key parameter

The `unique_key` is a column name or expression that is unique for the inputs of a snapshot. dbt uses [`unique_key`](https://docs.getdbt.com/reference/resource-configs/unique_key.md) to match records between a result set and an existing snapshot, so that changes can be captured correctly.

snapshots/orders.sql

```sql
{{ config(
  unique_key="column_name"
) }}
```

#### Examples[​](#examples-3 "Direct link to Examples")

* Using an `id` column as a unique key

  snapshots/orders.sql

  ```sql
  {{
      config(
        unique_key="id"
      )
  }}
  ```

  You can also write this in YAML. This might be a good idea if multiple snapshots share the same `unique_key` (though we prefer to apply this configuration in a config block, as above).

* #### Using a combination of two columns as a unique key[​](#using-a-combination-of-two-columns-as-a-unique-key "Direct link to Using a combination of two columns as a unique key")

  This configuration accepts a valid column expression. As such, you can concatenate two columns together as a unique key if required. It's a good idea to use a separator (like, '-') to ensure uniqueness.

  snapshots/transaction\_items\_snapshot.sql

  ```sql
  {% snapshot transaction_items_snapshot %}

      {{
          config(
            unique_key="transaction_id||'-'||line_item_id",
            ...
          )
      }}

  select
      transaction_id||'-'||line_item_id as id,
      *
  from {{ source('erp', 'transactions') }}

  {% endsnapshot %}
  ```

  Though, it's probably a better idea to construct this column in your query and use that as the `unique_key`:

  snapshots/transaction\_items\_snapshot.sql

  ```sql
  {% snapshot transaction_items_snapshot %}

      {{
          config(
            unique_key="id",
            ...
          )
      }}

  select
      transaction_id || '-' || line_item_id as id,
      *
  from {{ source('erp', 'transactions') }}

  {% endsnapshot %}
  ```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
