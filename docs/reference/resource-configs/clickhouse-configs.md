# ClickHouse configurations

## ClickHouse configurations[​](#clickhouse-configurations "Direct link to ClickHouse configurations")

### View materialization[​](#view-materialization "Direct link to View materialization")

A dbt model can be created as a [ClickHouse view](https://clickhouse.com/docs/en/sql-reference/table-functions/view/) and configured using the following syntax:

* Project YAML file
* SQL file config

dbt\_project.yml

```yaml
models:
  <resource-path>:
    +materialized: view
```

models/\<model\_name>.sql

```jinja
{{ config(materialized = "view") }}
```

### Table materialization[​](#table-materialization "Direct link to Table materialization")

A dbt model can be created as a [ClickHouse table](https://clickhouse.com/docs/en/operations/system-tables/tables/) and configured using the following syntax:

* Project YAML file
* SQL file config

dbt\_project.yml

```yaml
models:
  <resource-path>:
    +materialized: table
    +order_by: [ <column-name>, ... ]
    +engine: <engine-type>
    +partition_by: [ <column-name>, ... ]
```

models/\<model\_name>.sql

```jinja
{{ config(
    materialized = "table",
    engine = "<engine-type>",
    order_by = [ "<column-name>", ... ],
    partition_by = [ "<column-name>", ... ],
      ...
    ]
) }}
```

#### Table configuration[​](#table-configuration "Direct link to Table configuration")

| Option         | Description                                                                                                                                          | Required?                         |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------- |
| `materialized` | How the model will be materialized into ClickHouse. Must be `table` to create a table model.                                                         | Required                          |
| `engine`       | The table engine to use when creating tables. See list of supported engines below.                                                                   | Optional (default: `MergeTree()`) |
| `order_by`     | A tuple of column names or arbitrary expressions. This allows you to create a small sparse index that helps find data faster.                        | Optional (default: `tuple()`)     |
| `partition_by` | A partition is a logical combination of records in a table by a specified criterion. The partition key can be any expression from the table columns. | Optional                          |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

For the complete list of configuration options, see the [ClickHouse documentation](https://clickhouse.com/docs/integrations/dbt).

### Incremental materialization[​](#incremental-materialization "Direct link to Incremental materialization")

Table model will be reconstructed for each dbt execution. This may be infeasible and extremely costly for larger result sets or complex transformations. To address this challenge and reduce the build time, a dbt model can be created as an incremental ClickHouse table and is configured using the following syntax:

* Project file
* SQL file config

dbt\_project.yml

```yaml
models:
  <resource-path>:
    +materialized: incremental
    +order_by: [ <column-name>, ... ]
    +engine: <engine-type>
    +partition_by: [ <column-name>, ... ]
    +unique_key: [ <column-name>, ... ]
    +inserts_only: [ True|False ]
```

models/\<model\_name>.sql

```jinja
{{ config(
    materialized = "incremental",
    engine = "<engine-type>",
    order_by = [ "<column-name>", ... ],
    partition_by = [ "<column-name>", ... ],
    unique_key = [ "<column-name>", ... ],
    inserts_only = [ True|False ],
      ...
    ]
) }}
```

#### Incremental table configuration[​](#incremental-table-configuration "Direct link to Incremental table configuration")

| Option                   | Description                                                                                                                                                                                                                                                     | Required?                                                                            |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| `materialized`           | How the model will be materialized into ClickHouse. Must be `table` to create a table model.                                                                                                                                                                    | Required                                                                             |
| `unique_key`             | A tuple of column names that uniquely identify rows. For more details on uniqueness constraints, see [here](https://docs.getdbt.com/docs/build/incremental-models.md#defining-a-unique-key-optional).                                                           | Required. If not provided altered rows will be added twice to the incremental table. |
| `engine`                 | The table engine to use when creating tables. See list of supported engines below.                                                                                                                                                                              | Optional (default: `MergeTree()`)                                                    |
| `order_by`               | A tuple of column names or arbitrary expressions. This allows you to create a small sparse index that helps find data faster.                                                                                                                                   | Optional (default: `tuple()`)                                                        |
| `partition_by`           | A partition is a logical combination of records in a table by a specified criterion. The partition key can be any expression from the table columns.                                                                                                            | Optional                                                                             |
| `inserts_only`           | (Deprecated, see the `append` materialization strategy). If True, incremental updates will be inserted directly to the target incremental table without creating an intermediate table.                                                                         | Optional (default: `False`)                                                          |
| `incremental_strategy`   | The strategy to use for incremental materialization. `delete+insert`, `append` and `insert_overwrite` (experimental) are supported. For additional details on strategies, see [here](https://github.com/ClickHouse/dbt-clickhouse#incremental-model-strategies) | Optional (default: 'default')                                                        |
| `incremental_predicates` | Incremental predicate clause to be applied to `delete+insert` materializations                                                                                                                                                                                  | Optional                                                                             |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

For the complete list of configuration options, see the [ClickHouse documentation](https://clickhouse.com/docs/integrations/dbt).

## Snapshot[​](#snapshot "Direct link to Snapshot")

dbt snapshots allow a record to be made of changes to a mutable model over time. This in turn allows point-in-time queries on models, where analysts can “look back in time” at the previous state of a model. This functionality is supported by the ClickHouse connector and is configured using the following syntax:

<!-- -->

For more information on configuration, check out the [snapshot configs](https://docs.getdbt.com/reference/snapshot-configs.md) reference page.

## Learn more[​](#learn-more "Direct link to Learn more")

The `dbt-clickhouse` adapter supports most dbt-native features like tests, snapshots, helper macros, and more. For a complete overview of supported features and best practices, see the [ClickHouse documentation](https://clickhouse.com/docs/integrations/dbt).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
