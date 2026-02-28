# freshness

* Project file
* Model YAML

dbt\_project.yml

```yaml
sources:
  <resource-path>:
    +freshness:
      warn_after:  
        count: <positive_integer>
        period: minute | hour | day
```

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
      # or use loaded_at_query in v1.10 or higher. Should not be used if loaded_at_field is defined
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
          # changed to config in v1.10
          loaded_at_field: <column_name_or_expression>
          # or use loaded_at_query in v1.10 or higher. Should not be used if loaded_at_field is defined
          loaded_at_query: <sql_expression>

        ...
```

## Definition[​](#definition "Direct link to Definition")

A freshness block is used to define the acceptable amount of time between the most recent record, and now, for a table to be considered "fresh".

In the `freshness` block, one or both of `warn_after` and `error_after` can be provided. If neither is provided, then dbt will not calculate freshness snapshots for the tables in this source.

* `warn_after`: Duration (for example, 12 hours) after which dbt raises a warning if the most recent available data is older than this threshold.
* `error_after`: Duration (for example, 24 hours) after which dbt fails the freshness check if the most recent available data is older than this threshold.

In most cases, the `loaded_at_field` is required. Some adapters support calculating source freshness from the warehouse metadata tables and can exclude the `loaded_at_field`.

If a source has a `freshness:` block, dbt will attempt to calculate freshness for that source:

* If `loaded_at_field` is provided, dbt will calculate freshness via a select query.
* If `loaded_at_field` is *not* provided, dbt will calculate freshness via warehouse metadata tables when possible.

<!-- -->

Currently, calculating freshness from warehouse metadata tables is supported on the following adapters:

* [Snowflake](https://docs.getdbt.com/reference/resource-configs/snowflake-configs.md)
* [Redshift](https://docs.getdbt.com/reference/resource-configs/redshift-configs.md)
* [BigQuery](https://docs.getdbt.com/reference/resource-configs/bigquery-configs.md) (Supported in [`dbt-bigquery`](https://github.com/dbt-labs/dbt-bigquery) version 1.7.3 or higher)
* [Databricks](https://docs.getdbt.com/reference/resource-configs/databricks-configs.md) (Supported in the dbt Fusion engine)

Freshness blocks are applied hierarchically:

* A `freshness` and `loaded_at_field` property added to a source will be applied to all tables defined in that source.
* A `freshness` and `loaded_at_field` property added to a source *table* will override any properties applied to the source.

This is useful when all of the tables in a source have the same `loaded_at_field`, as is often the case.

To exclude a source from freshness calculations, explicitly set `freshness: null`.

In state-aware orchestration, dbt uses the warehouse metadata by default to check if sources (or upstream models in the case of Mesh) are fresh. For more information on how freshness is used by state-aware orchestration, see [Advanced configurations](https://docs.getdbt.com/docs/deploy/state-aware-setup.md#advanced-configurations).

## loaded\_at\_field[​](#loaded_at_field "Direct link to loaded_at_field")

Optional on adapters that support pulling freshness from warehouse metadata tables, required otherwise.<br /><br />A column name (or expression) that returns a timestamp indicating freshness.

Examples:

```yml
sources:
  - name: inventory_updates
    config:
      freshness:
        error_after:
          count: 24
          period: hour
      loaded_at_field: updated_at
```

If using a date field, you may have to cast it to a timestamp:

```yml
sources:
  - name: work_orders
    description: |
      Work orders from ERP. The completed_date column is stored as DATE but we need to compare it as a timestamp for freshness checks.
    config:
      freshness:
        error_after:
          count: 24
          period: hour
      loaded_at_field: "completed_date::timestamp"
```

Or, depending on your SQL variant:

```yml
sources:
  - name: purchase_orders
    description: |
      Purchase orders. The completed_date is stored as VARCHAR in 'YYYY-MM-DD' format. Use CAST for explicit conversion.
    config:
      freshness:
        error_after:
          count: 24
          period: hour
      loaded_at_field: "CAST(completed_date AS TIMESTAMP)"
```

If using a non-UTC timestamp, cast it to UTC first:

```yml
sources:
  - name: customer_transactions
    description: |
      Customer transactions recorded in Sydney local time. Converting to UTC for consistent freshness comparison across sources in different timezones.
    config:
      freshness:
        error_after:
          count: 24
          period: hour
      loaded_at_field: "convert_timezone('Australia/Sydney', 'UTC', created_at_local)"
```

<!-- -->

## count[​](#count "Direct link to count")

(Required)

A positive integer for the number of periods where a data source is still considered "fresh".

## period[​](#period "Direct link to period")

(Required)

The time period used in the freshness calculation. One of `minute`, `hour` or `day`

## filter[​](#filter "Direct link to filter")

(optional)

Add a where clause to the query run by `dbt source freshness` in order to limit data scanned.

This filter *only* applies to dbt's source freshness queries - it will not impact other uses of the source table.

This is particularly useful if:

* You are using BigQuery and your source tables are [partitioned tables](https://cloud.google.com/bigquery/docs/partitioned-tables)
* You are using Snowflake, Databricks, or Spark with large tables, and this results in a performance benefit

## Examples[​](#examples "Direct link to Examples")

### Complete example[​](#complete-example "Direct link to Complete example")

models/\<filename>.yml

```yaml


sources:
  - name: jaffle_shop
    database: raw
    config: 
      # changed to config in v1.9
      freshness: # default freshness
        warn_after: {count: 12, period: hour}
        error_after: {count: 24, period: hour}

      loaded_at_field: _etl_loaded_at

    tables:
      - name: customers # this will use the freshness defined above

      - name: orders
        config:
          freshness: # make this a little more strict
            warn_after: {count: 6, period: hour}
            error_after: {count: 12, period: hour}
            # Apply a where clause in the freshness query
            filter: datediff('day', _etl_loaded_at, current_timestamp) < 2


      - name: product_skus
        config:
          freshness: # do not check freshness for this table
```

When running `dbt source freshness`, the following query will be run:

* Compiled SQL
* Jinja SQL

```sql
select
  max(_etl_loaded_at) as max_loaded_at,
  convert_timezone('UTC', current_timestamp()) as snapshotted_at
from raw.jaffle_shop.orders

where datediff('day', _etl_loaded_at, current_timestamp) < 2
```

```sql
select
  max({{ loaded_at_field }}) as max_loaded_at,
  {{ current_timestamp() }} as snapshotted_at
from {{ source }}
{% if filter %}
where {{ filter }}
{% endif %}
```

*[Source code](https://github.com/dbt-labs/dbt-core/blob/HEAD/core/dbt/include/global_project/macros/adapters/common.sql#L262)*

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
