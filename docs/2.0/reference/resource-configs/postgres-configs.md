# Postgres configurations

## Incremental materialization strategies[​](#incremental-materialization-strategies "Direct link to Incremental materialization strategies")

In dbt-postgres, the following incremental materialization strategies are supported:

* `append` (default when `unique_key` is not defined)
* `merge`
* `delete+insert` (default when `unique_key` is defined)
* [`microbatch`](https://docs.getdbt.com/docs/build/incremental-microbatch.md)

## Performance optimizations[​](#performance-optimizations "Direct link to Performance optimizations")

### Unlogged[​](#unlogged "Direct link to Unlogged")

"Unlogged" tables can be considerably faster than ordinary tables, as they are not written to the write-ahead log nor replicated to read replicas. They are also considerably less safe than ordinary tables. See [Postgres docs](https://www.postgresql.org/docs/current/sql-createtable.html#SQL-CREATETABLE-UNLOGGED) for details.

my\_table.sql

```sql
{{ config(materialized='table', unlogged=True) }}

select ...
```

dbt\_project.yml

```yaml
models:
  +unlogged: true
```

### Indexes[​](#indexes "Direct link to Indexes")

While Postgres works reasonably well for datasets smaller than about 10m rows, database tuning is sometimes required. It's important to create indexes for columns that are commonly used in joins or where clauses.

Table models, incremental models, seeds, snapshots, and materialized views may have a list of `indexes` defined. Each Postgres index can have three components:

* `columns` (list, required): one or more columns on which the index is defined
* `unique` (boolean, optional): whether the index should be [declared unique](https://www.postgresql.org/docs/9.4/indexes-unique.html)
* `type` (string, optional): a supported [index type](https://www.postgresql.org/docs/current/indexes-types.html) (B-tree, Hash, GIN, etc)

my\_table.sql

```sql
{{ config(
    materialized = 'table',
    indexes=[
      {'columns': ['column_a'], 'type': 'hash'},
      {'columns': ['column_a', 'column_b'], 'unique': True},
    ]
)}}

select ...
```

If one or more indexes are configured on a resource, dbt will run `create index` DDL statement(s) as part of that resource's materialization, within the same transaction as its main `create` statement. For the index's name, dbt uses a hash of its properties and the current timestamp, in order to guarantee uniqueness and avoid namespace conflict with other indexes.

```sql
create index if not exists
"3695050e025a7173586579da5b27d275"
on "my_target_database"."my_target_schema"."indexed_model" 
using hash
(column_a);

create unique index if not exists
"1bf5f4a6b48d2fd1a9b0470f754c1b0d"
on "my_target_database"."my_target_schema"."indexed_model" 
(column_a, column_b);
```

You can also configure indexes for a number of resources at once:

dbt\_project.yml

```yaml
models:
  project_name:
    subdirectory:
      +indexes:
        - columns: ['column_a']
          type: hash
```

## Materialized views[​](#materialized-views "Direct link to Materialized views")

The Postgres adapter supports [materialized views](https://www.postgresql.org/docs/current/rules-materializedviews.html) with the following configuration parameters:

| Parameter                                                                                                  | Type               | Required | Default | Change Monitoring Support |
| ---------------------------------------------------------------------------------------------------------- | ------------------ | -------- | ------- | ------------------------- |
| [`on_configuration_change`](https://docs.getdbt.com/reference/resource-configs/on_configuration_change.md) | `<string>`         | no       | `apply` | n/a                       |
| [`indexes`](#indexes)                                                                                      | `[{<dictionary>}]` | no       | `none`  | alter                     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

* Project YAML file
* Properties YAML file
* SQL file config

dbt\_project.yml

```yaml
models:
  <resource-path>:
    +materialized: materialized_view
    +on_configuration_change: apply | continue | fail
    +indexes:
      - columns: [<column-name>]
        unique: true | false
        type: hash | btree
```

models/properties.yml

```yaml

models:
  - name: [<model-name>]
    config:
      materialized: materialized_view
      on_configuration_change: apply | continue | fail
      indexes:
        - columns: [<column-name>]
          unique: true | false
          type: hash | btree
```

models/\<model\_name>.sql

```jinja
{{ config(
    materialized="materialized_view",
    on_configuration_change="apply" | "continue" | "fail",
    indexes=[
        {
            "columns": ["<column-name>"],
            "unique": true | false,
            "type": "hash" | "btree",
        }
    ]
) }}
```

The [`indexes`](#indexes) parameter corresponds to that of a table, as explained above. It's worth noting that, unlike tables, dbt monitors this parameter for changes and applies the changes without dropping the materialized view. This happens via a `DROP/CREATE` of the indexes, which can be thought of as an `ALTER` of the materialized view.

Learn more about these parameters in Postgres's [docs](https://www.postgresql.org/docs/current/sql-creatematerializedview.html).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
