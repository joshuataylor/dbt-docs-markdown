# sql\_header

`sql_header` does not support Jinja or macros like `ref` and `source`

Unlike [pre-hooks and post-hooks](https://docs.getdbt.com/reference/resource-configs/pre-hook-post-hook.md), macros like [`ref`](https://docs.getdbt.com/reference/dbt-jinja-functions/ref.md), [`source`](https://docs.getdbt.com/reference/dbt-jinja-functions/source.md), and references like [`{{ this }}`](https://docs.getdbt.com/reference/dbt-jinja-functions/this.md), aren't supported.

The primary function of `set_sql_header` is fairly limited. It's intended to:

* [Create UDFs](https://docs.getdbt.com/reference/resource-configs/sql_header.md#create-a-bigquery-temporary-udf)
* [Set script variables](https://cloud.google.com/bigquery/docs/reference/standard-sql/procedural-language) (BigQuery)
* [Set temporary session parameters](https://docs.getdbt.com/reference/resource-configs/sql_header.md#set-snowflake-session-parameters-for-a-particular-model) (Snowflake)

- Models
- Seeds
- Snapshots

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

This config is not implemented for seeds

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

## Definition[​](#definition "Direct link to Definition")

An optional configuration to inject SQL above the `create table as` and `create view as` statements that dbt executes when building models and snapshots.

`sql_header`s can be set using the config, or by `call`-ing the `set_sql_header` macro (example below).

## Comparison to pre-hooks[​](#comparison-to-pre-hooks "Direct link to Comparison to pre-hooks")

[Pre-hooks](https://docs.getdbt.com/reference/resource-configs/pre-hook-post-hook.md) also provide an opportunity to execute SQL before model creation, as a *preceding* query. In comparison, SQL in a `sql_header` is run in the same *query* as the `create table|view as` statement.

As a result, this makes it more useful for [Snowflake session parameters](https://docs.snowflake.com/en/sql-reference/parameters.html) and [BigQuery Temporary UDFs](https://cloud.google.com/bigquery/docs/reference/standard-sql/user-defined-functions#sql-udf-examples).

## Examples[​](#examples "Direct link to Examples")

### Set Snowflake session parameters for a particular model[​](#set-snowflake-session-parameters-for-a-particular-model "Direct link to Set Snowflake session parameters for a particular model")

This uses the config block syntax:

models/my\_model.sql

```sql
{{ config(
  sql_header="alter session set timezone = 'Australia/Sydney';"
) }}

select * from {{ ref('other_model') }}
```

### Set Snowflake session parameters for all models[​](#set-snowflake-session-parameters-for-all-models "Direct link to Set Snowflake session parameters for all models")

dbt\_project.yml

```yml
config-version: 2

models:
  +sql_header: "alter session set timezone = 'Australia/Sydney';"
```

### Create a BigQuery Temporary UDF[​](#create-a-bigquery-temporary-udf "Direct link to Create a BigQuery Temporary UDF")

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

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
