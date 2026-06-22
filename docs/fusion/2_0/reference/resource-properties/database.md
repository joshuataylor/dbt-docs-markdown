# Defining a database source property

models/\<filename>.yml

```yml

sources:
  - name: <source_name>
    database: <database_name>
    tables:
      - name: <table_name>
      - ...
```

## Definition[​](#definition "Direct link to Definition")

The database that your source is stored in.

Note that to use this parameter, your warehouse must allow cross-database queries.

#### BigQuery terminology[​](#bigquery-terminology "Direct link to BigQuery terminology")

If you're using BigQuery, use the *project* name as the `database:` property.

## Default[​](#default "Direct link to Default")

By default, dbt will search in your target database (i.e. the database that you are creating tables and views).

## Examples[​](#examples "Direct link to Examples")

### Define a source that is stored in the `raw` database[​](#define-a-source-that-is-stored-in-the-raw-database "Direct link to define-a-source-that-is-stored-in-the-raw-database")

models/\<filename>.yml

```yml

sources:
  - name: jaffle_shop
    database: raw
    tables:
      - name: orders
      - name: customers
```
