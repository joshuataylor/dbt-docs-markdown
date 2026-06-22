# identifier

models/\<filename>.yml

```yml

sources:
  - name: <source_name>
    database: <database_name>
    tables:
      - name: <table_name>
        identifier: <table_identifier>
```

## Definition[​](#definition "Direct link to Definition")

The table name as stored in the database.

This parameter is useful if you want to use a source table name that differs from the table name in the database.

## Default[​](#default "Direct link to Default")

By default, dbt will use the table's `name` parameter as the identifier.

## Examples[​](#examples "Direct link to Examples")

### Use a simpler name for a source table than the one in your database[​](#use-a-simpler-name-for-a-source-table-than-the-one-in-your-database "Direct link to Use a simpler name for a source table than the one in your database")

models/\<filename>.yml

```yml

sources:
  - name: jaffle_shop
    tables:
      - name: orders
        identifier: api_orders
```

In a downstream model:

```sql
select * from {{ source('jaffle_shop', 'orders') }}
```

Will get compiled to:

```sql
select * from jaffle_shop.api_orders
```

### Reference sharded tables as a source in BigQuery[​](#reference-sharded-tables-as-a-source-in-bigquery "Direct link to Reference sharded tables as a source in BigQuery")

models/\<filename>.yml

```yml

sources:
  - name: ga
    tables:
      - name: events
        identifier: "events_*"
```

In a downstream model:

```sql
select * from {{ source('ga', 'events') }}

-- filter on shards by suffix
where _table_suffix > '20200101'
```

Will get compiled to:

```sql
select * from `my_project`.`ga`.`events_*`

-- filter on shards by suffix
where _table_suffix > '20200101'
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
