# sql\_header

`sql_header` does not support Jinja or macros like `ref` and `source`

Unlike [pre-hooks and post-hooks](./pre-hook-post-hook.md), macros like [`ref`](../dbt-jinja-functions/ref.md), [`source`](../dbt-jinja-functions/source.md), and references like [`{{ this }}`](../dbt-jinja-functions/this.md), aren't supported.

The primary function of `set_sql_header` is fairly limited. It's intended to:

* [Create UDFs](./sql_header.md#create-a-bigquery-temporary-udf)
* [Set script variables](https://cloud.google.com/bigquery/docs/reference/standard-sql/procedural-language) (BigQuery)
* [Set temporary session parameters](./sql_header.md#set-snowflake-session-parameters-for-a-particular-model) (Snowflake)

### Models

models/\<modelname>.sql

```sql
{{ config(
  sql_header="<sql-statement>"
) }}

select ...
```

dbt\_project.yml

```yml
config-version: 2

models:
  <resource-path>:
    +sql_header: <sql-statement>
```

### Seeds

This config is not implemented for seeds

### Snapshots

snapshots/\<filename>.sql

```sql
{% snapshot snapshot_name %}

{{ config(
  sql_header="<sql-statement>"
) }}

select ...

{% endsnapshot %}
```

dbt\_project.yml

```yml
snapshots:
  <resource-path>:
    +sql_header: <sql-statement>
```

### Property file

Setting `sql_header` in the `config` of a [generic data test](../../docs/build/data-tests.md) is available starting in dbt Core v1.12. Enable the [`require_sql_header_in_test_configs`](../global-configs/behavior-flags/require_sql_header_in_test_configs.md) flag to use `sql_header` in `properties.yml` for generic data tests.

Here's an example of a model-level configuration:

models/properties.yml

```yaml
models:
  - name: orders
    data_tests:
      - unique:
          name: unique_orders_order_id
          arguments:
            column_name: order_id
          config:
            sql_header: "-- SQL_HEADER_TEST_MARKER"
```

You can also use `sql_header` for column-level data tests:

models/properties.yml

```yaml
models:
  - name: orders
    columns:
      - name: order_id
        data_tests:
          - not_null:
              name: not_null_orders_order_id
              config:
                sql_header: "-- SQL_HEADER_TEST_MARKER"
```

## Definition

An optional configuration to inject SQL above the `create table as` and `create view as` statements that dbt executes when building models and snapshots.

`sql_header`s can be set using the config, or by `call`-ing the `set_sql_header` macro (example below).

## Comparison to pre-hooks

[Pre-hooks](./pre-hook-post-hook.md) also provide an opportunity to execute SQL before model creation, as a *preceding* query. In comparison, SQL in a `sql_header` is run in the same *query* as the `create table|view as` statement.

As a result, this makes it more useful for [Snowflake session parameters](https://docs.snowflake.com/en/sql-reference/parameters.html) and [BigQuery Temporary UDFs](https://cloud.google.com/bigquery/docs/reference/standard-sql/user-defined-functions#sql-udf-examples).

## Examples

### Set Snowflake session parameters for a particular model

This uses the config block syntax:

models/my\_model.sql

```sql
{{ config(
  sql_header="alter session set timezone = 'Australia/Sydney';"
) }}

select * from {{ ref('other_model') }}
```

### Set Snowflake session parameters for all models

dbt\_project.yml

```yml
config-version: 2

models:
  +sql_header: "alter session set timezone = 'Australia/Sydney';"
```

### Create a BigQuery Temporary UDF

This example calls the `set_sql_header` macro. This macro is a convenience wrapper which you may choose to use if you have a multi-line SQL statement to inject. You do not need to use the `sql_header` configuration key in this case.

models/my\_model.sql

```sql
-- Supply a SQL header:
{% call set_sql_header(config) %}
  CREATE TEMPORARY FUNCTION yes_no_to_boolean(answer STRING)
  RETURNS BOOLEAN AS (
    CASE
    WHEN LOWER(answer) = 'yes' THEN True
    WHEN LOWER(answer) = 'no' THEN False
    ELSE NULL
    END
  );
{%- endcall %}

-- Supply your model code:


select yes_no_to_boolean(yes_no) from {{ ref('other_model') }}
```
