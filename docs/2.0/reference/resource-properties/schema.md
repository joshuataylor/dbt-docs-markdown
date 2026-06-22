# Defining a schema source property

models/\<filename>.yml

```yml

sources:
  - name: <source_name>
    database: <database_name>
    schema: <schema_name>
    tables:
      - name: <table_name>
      - ...
```

## Definition[​](#definition "Direct link to Definition")

The schema name as stored in the database.

This parameter is useful if you want to use a [source](https://docs.getdbt.com/reference/source-properties.md) name that differs from the schema name.

BigQuery terminology

If you're using BigQuery, use the *dataset* name as the `schema` property.

## Default[​](#default "Direct link to Default")

By default, dbt will use the source's `name` parameter as the schema name.

## Examples[​](#examples "Direct link to Examples")

### Use a simpler name for a source schema than the one in your database[​](#use-a-simpler-name-for-a-source-schema-than-the-one-in-your-database "Direct link to Use a simpler name for a source schema than the one in your database")

models/\<filename>.yml

```yml

sources:
  - name: jaffle_shop
    schema: postgres_backend_public_schema
    tables:
      - name: orders
```

In a downstream model:

```sql
select * from {{ source('jaffle_shop', 'orders') }}
```

Will get compiled to:

```sql
select * from postgres_backend_public_schema.orders
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
