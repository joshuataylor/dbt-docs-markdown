# freshness

Use the `freshness` config to declare how fresh your [source](#source-freshness) or [model](#model-freshness) data should be.

(Applies to dbt v1.99 and earlier)

Run [`dbt source freshness`](../commands/source.md) to check your sources. Model freshness configuration is only available in dbt v2. Refer to [Source freshness](#source-freshness) for more information.

## Configuration

Use the following fields to configure freshness for sources and models unless otherwise noted:

| Field             | Description                                                                                                                                                                                                                                            |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `warn_after`      | How old the most recent data can be before a freshness check reports a warning. Requires both `count` and `period`.                                                                                                                                    |
| `error_after`     | How old the most recent data can be before a freshness check reports an error. Same format as `warn_after`.                                                                                                                                            |
| `loaded_at_field` | Column dbt queries to determine the most recent loaded timestamp. Required when adapter metadata is unavailable.                                                                                                                                       |
| `loaded_at_query` | A SQL expression that returns the most recent loaded timestamp. Alternative to `loaded_at_field`. Setting both `loaded_at_query` and `loaded_at_field` on the same resource is a parse error. Available in dbt v1.10 and later.                        |
| `filter`          | Adds a `WHERE` clause to the freshness query to limit data scanned. Useful for BigQuery partitioned tables or large tables on Snowflake, Databricks, or Spark. Does not affect other uses of the source or model. Does not apply to `loaded_at_query`. |

One or both of `warn_after` and `error_after` can be provided. If neither is set, dbt will not check freshness for that resource. Each of `warn_after` and `error_after` requires both `count` and `period`; setting only one issues a parse warning but causes an error when `dbt freshness` runs.

## Source freshness

### Project file

dbt\_project.yml

```yaml
sources:
  <resource-path>:
    +freshness:
      warn_after:
        count: <positive_integer>
        period: minute | hour | day
```

### Property file

models/\<filename>.yml

```yaml
sources:
  - name: <source_name>
    config:
      freshness: # changed to config in v1.9
        warn_after:
          count: <positive_integer>
          period: minute | hour | day
        error_after:
          count: <positive_integer>
          period: minute | hour | day
        filter: <boolean_sql_expression>
      # changed to config in v1.10
      loaded_at_field: <column_name_or_expression>
      # or use loaded_at_query in v1.10 or higher
      loaded_at_query: <sql_expression>

    tables:
      - name: <table_name>
        config:
          # source.table.config.freshness overrides source.config.freshness
          freshness:
            warn_after:
              count: <positive_integer>
              period: minute | hour | day
            error_after:
              count: <positive_integer>
              period: minute | hour | day
            filter: <boolean_sql_expression>
          loaded_at_field: <column_name_or_expression>
          loaded_at_query: <sql_expression>
```

Freshness blocks are applied hierarchically:

* A `freshness` and `loaded_at_field` set on a source apply to all tables in that source.
* A `freshness` and `loaded_at_field` set on a source *table* override the source-level values.

To exclude a source from freshness calculations, explicitly set `freshness: null`.

If a source has a `freshness:` block, dbt will attempt to calculate freshness for that source:

* If `loaded_at_field` is provided, dbt calculates freshness via a select query.
* If `loaded_at_field` is *not* provided, dbt calculates freshness via warehouse metadata tables when possible.

(Applies to dbt v1.99 and earlier)

Currently, calculating freshness from warehouse metadata tables is supported on the following adapters:

* [Snowflake](./snowflake-configs.md)
* [Redshift](./redshift-configs.md)
* [BigQuery](./bigquery-configs.md) (requires [`dbt-bigquery`](https://github.com/dbt-labs/dbt-bigquery) version 1.7.3 or higher)
* [Databricks](./databricks-configs.md)

### Examples

#### Using `loaded_at_field`

```yml
sources:
  - name: jaffle_shop
    # Cast a date field to timestamp
    loaded_at_field: "completed_date::timestamp"

    tables:
      - name: orders
        # Cast a non-UTC timestamp to UTC
        loaded_at_field: "convert_timezone('Australia/Sydney', 'UTC', created_at_local)"
        config:
          freshness:
            warn_after: {count: 12, period: hour}
```

#### Using `loaded_at_query`

(Applies to dbt v1.10 and later)

```yaml
sources:
  - name: jaffle_shop
    tables:
      - name: orders
        loaded_at_query: |
          select max(_sdc_batched_at) from (
            select * from {{ this }}
            where _sdc_batched_at > dateadd(day, -7, current_date)
            qualify count(*) over (partition by _sdc_batched_at::date) > 2000
          )
        config:
          freshness:
            warn_after: {count: 12, period: hour}
```

#### Complete example

models/\<filename>.yml

```yaml
sources:
  - name: jaffle_shop
    database: raw
    config:
      freshness: # default freshness for all tables
        warn_after: {count: 12, period: hour}
        error_after: {count: 24, period: hour}
      loaded_at_field: _etl_loaded_at

    tables:
      - name: customers # uses the freshness defined above

      - name: orders
        config:
          freshness: # more strict for orders
            warn_after: {count: 6, period: hour}
            error_after: {count: 12, period: hour}
            filter: datediff('day', _etl_loaded_at, current_timestamp) < 2

      - name: product_skus
        config:
          freshness: null # do not check freshness for this table
```

(Applies to dbt v1.99 and earlier)

When running [`dbt source freshness`](../commands/source.md), the following query will be run against the `orders` table:

##### Compiled SQL

```sql
select
  max(_etl_loaded_at) as max_loaded_at,
  convert_timezone('UTC', current_timestamp()) as snapshotted_at
from raw.jaffle_shop.orders
where datediff('day', _etl_loaded_at, current_timestamp) < 2
```

##### Jinja SQL

```sql
select
  max({{ loaded_at_field }}) as max_loaded_at,
  {{ current_timestamp() }} as snapshotted_at
from {{ source }}
{% if filter %}
where {{ filter }}
{% endif %}
```

*[Source code](https://github.com/dbt-labs/dbt-adapters/blob/main/dbt-adapters/src/dbt/include/global_project/macros/adapters/freshness.sql#L5-L16)*
