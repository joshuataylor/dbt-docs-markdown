# Snapshot configurations

Learn about using snapshot configurations in dbt, including snapshot-specific configurations and general configurations.

## Related documentation

* [Snapshots](../docs/build/snapshots.md)
* The `dbt snapshot` [command](./commands/snapshot.md)

Learn by video!

For video tutorials on Snapshots, go to dbt Learn and check out the [Snapshots course](https://learn.getdbt.com/courses/snapshots).

## Available configurations

### Snapshot-specific configurations

Resource-specific configurations are applicable to only one dbt resource type rather than multiple resource types. You can define these settings in the project file (`dbt_project.yml`), a property file (`models/properties.yml` for models, similarly for other resources), or within the resource’s file using the `{{ config() }}` macro.<br />

The following resource-specific configurations are only available to Snapshots:

### Project file

(Applies to dbt v1.9 and later)

dbt\_project.yml

```yaml
snapshots:
  <resource-path>:
    +schema: <string>
    +database: <string>
    +alias: <string>
    +unique_key: <column_name_or_expression>
    +strategy: timestamp | check
    +updated_at: <column_name>
    +check_cols: [<column_name>] | all
    +snapshot_meta_column_names: {<dictionary>}
    +dbt_valid_to_current: <string> 
    +hard_deletes: string
```

### Property file

(Applies to dbt v1.9 and later)

Refer to [configuring snapshots](../docs/build/snapshots.md#configuring-snapshots) for the available configurations.

snapshots/schema.yml

```yml
snapshots:
  - name: <string>
    relation: ref() | source()
    config:
      database: <string>
      schema: <string>
      unique_key: <column_name_or_expression>
      strategy: timestamp | check
      updated_at: <column_name>
      check_cols: [<column_name>] | all
      snapshot_meta_column_names: {<dictionary>}
      hard_deletes: string
      dbt_valid_to_current: <string>
```

### SQL file config

info

Starting from [the dbt **Latest** release track](../docs/dbt-versions/dbt-release-tracks.md) and dbt Core v1.9, defining snapshots in a `.sql` file using a config block is a legacy method. You can define snapshots in properties YAML files using the latest [snapshot-specific configurations](../docs/build/snapshots.md#configuring-snapshots). For new snapshots, we recommend using these latest configs. If applying them to existing snapshots, you'll need to [migrate](#snapshot-configuration-migration) over.

### Snapshot configuration migration

The latest snapshot configurations introduced in dbt Core v1.9 (such as [`snapshot_meta_column_names`](./resource-configs/snapshot_meta_column_names.md), [`dbt_valid_to_current`](./resource-configs/dbt_valid_to_current.md), and `hard_deletes`) are best suited for new snapshots, but you can also adopt them in existing snapshots by migrating your table schema and configs carefully to avoid any inconsistencies in your snapshots.

Here's how you can do it:

1. In your data platform, create a backup snapshot table. You can copy it to a new table:

   ```sql
   create table my_snapshot_table_backup as
   select * from my_snapshot_table;
   ```

   This allows you to restore your snapshot if anything goes wrong during migration.

2. If you want to use the new configs, add required columns to your existing snapshot table using `alter` statements as needed. Here's an example of what to add if you're going to use `dbt_valid_to_current` and `snapshot_meta_column_names`:

   ```sql
   alter table my_snapshot_table
   add column dbt_valid_from timestamp,
   add column dbt_valid_to timestamp;
   ```

3. Then update your snapshot config:

   ```yaml
   snapshots:
     - name: orders_snapshot
       relation: source('something','orders')
       config:
         strategy: timestamp
         updated_at: updated_at
         unique_key: id
         dbt_valid_to_current: "to_date('9999-12-31')"
         snapshot_meta_column_names:
           dbt_valid_from: start_date
           dbt_valid_to: end_date
   ```

4. Test each change before adopting multiple new configs by running `dbt snapshot` in development or staging.

5. Confirm if the snapshot run completes without errors, the new columns are created, and historical logic behaves as you’d expect. The table should look like this:

   | `id` | `start_date`        | `end_date`          | `updated_at`        |
   | ---- | ------------------- | ------------------- | ------------------- |
   | 1    | 2024-10-01 09:00:00 | 2024-10-03 08:00:00 | 2024-10-01 09:00:00 |
   | 2    | 2024-10-03 08:00:00 | 9999-12-31 00:00:00 | 2024-10-03 08:00:00 |
   | 3    | 2024-10-02 11:15:00 | 9999-12-31 00:00:00 | 2024-10-02 11:15:00 |

Note: The `end_date` column (defined by `snapshot_meta_column_names`) uses the configured value from `dbt_valid_to_current` (9999-12-31) for newly inserted records, instead of the default `NULL`. Existing records will have `NULL` for `end_date`.

warning

If you use one of the latest configs, such as `dbt_valid_to_current`, without migrating your data, you may have mixed old and new data, leading to an incorrect downstream result.

### General configurations

General configurations provide broader operational settings applicable across multiple resource types. Like resource-specific configurations, these can also be set in the project file, property files, or within resource-specific files.

### Project file

dbt\_project.yml

(Applies to dbt v1.9 and later)

```yaml
snapshots:
  <resource-path>:
    +enabled: true | false
    +tags: <string> | [<string>]
    +alias: <string>
    +pre-hook: <sql-statement> | [<sql-statement>]
    +post-hook: <sql-statement> | [<sql-statement>]
    +persist_docs: {<dict>}
    +grants: {<dict>}
    +event_time: my_time_field
```

### Property file

(Applies to dbt v1.9 and later)

snapshots/properties.yml

```yaml

snapshots:
  - name: [<snapshot-name>]
    relation: source('my_source', 'my_table')
    config:
      enabled: true | false
      tags: <string> | [<string>]
      alias: <string>
      pre_hook: <sql-statement> | [<sql-statement>]
      post_hook: <sql-statement> | [<sql-statement>]
      persist_docs: {<dict>}
      grants: {<dictionary>}
      event_time: my_time_field
```

### SQL file config

info

Starting from [the dbt **Latest** release track](../docs/dbt-versions/dbt-release-tracks.md) and dbt Core v1.9, defining snapshots in a `.sql` file using a config block is a legacy method. You can define snapshots in properties YAML files using the latest [snapshot-specific configurations](../docs/build/snapshots.md#configuring-snapshots). For new snapshots, we recommend using these latest configs. If applying them to existing snapshots, you'll need to [migrate](#snapshot-configuration-migration) over.

## Configuring snapshots

Snapshots can be configured in multiple ways:

(Applies to dbt v1.9 and later)

1. Defined in YAML files using the `config` [resource property](./model-properties.md), typically in your [snapshots directory](./project-configs/snapshot-paths.md) or whichever folder you prefer. Available in [the dbt release track](../docs/dbt-versions/dbt-release-tracks.md), dbt v1.9 and higher.
2. From the `dbt_project.yml` file, under the `snapshots:` key. To apply a configuration to a snapshot, or directory of snapshots, define the resource path as nested dictionary keys.

Snapshot configurations are applied hierarchically in the order above with higher taking precedence. You can also apply [data tests](./snapshot-properties.md) to snapshots using the [`tests` property](./resource-properties/data-tests.md).

### Examples

(Applies to dbt v1.9 and later)

The following examples demonstrate how to configure snapshots using the `dbt_project.yml` file and a `.yml` file.

* #### Apply configurations to all snapshots

  To apply a configuration to all snapshots, including those in any installed [packages](../docs/build/packages.md), nest the configuration directly under the `snapshots` key:

  dbt\_project.yml

  ```yml
  snapshots:
    +unique_key: id
  ```

* #### Apply configurations to all snapshots in your project

  To apply a configuration to all snapshots in your project only (for example, *excluding* any snapshots in installed packages), provide your project name as part of the resource path.

  For a project named `jaffle_shop`:

  dbt\_project.yml

  ```yml
  snapshots:
    jaffle_shop:
      +unique_key: id
  ```

  Similarly, you can use the name of an installed package to configure snapshots in that package.

* #### Apply configurations to one snapshot only

  (Applies to dbt v1.9 and later)

  snapshots/postgres\_app/order\_snapshot.yml

  ```yaml
  snapshots:
   - name: orders_snapshot
     relation: source('jaffle_shop', 'orders')
     config:
       unique_key: id
       strategy: timestamp
       updated_at: updated_at
       persist_docs:
         relation: true
         columns: true
  ```

  Pro-tip: Use sources in snapshots: `select * from {{ source('jaffle_shop', 'orders') }}`

  You can also use the full resource path (including the project name, and subdirectories) to configure an individual snapshot from your `dbt_project.yml` file.

  For a project named `jaffle_shop`, with a snapshot file within the `snapshots/postgres_app/` directory, where the snapshot is named `orders_snapshot` (as above), this would look like:

  dbt\_project.yml

  ```yml
  snapshots:
    jaffle_shop:
      postgres_app:
        orders_snapshot:
          +unique_key: id
          +strategy: timestamp
          +updated_at: updated_at
  ```

  You can also define some common configs in a snapshot's `config` block. However, we don't recommend this for a snapshot's required configuration.

  dbt\_project.yml

  ```yml

  snapshots:
    - name: orders_snapshot
      +persist_docs:
        relation: true
        columns: true
  ```
