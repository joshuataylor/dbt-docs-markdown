# What if my source is in a poorly named schema or table?

By default, dbt will use the `name:` parameters to construct the source reference.

If these names are a little less-than-perfect, use the [schema](https://docs.getdbt.com/reference/resource-properties/schema.md) and [identifier](https://docs.getdbt.com/reference/resource-properties/identifier.md) properties to define the names as per the database, and use your `name:` property for the name that makes sense!

models/\<filename>.yml

```yml
sources:
  - name: jaffle_shop
    database: raw
    schema: postgres_backend_public_schema
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
select * from raw.postgres_backend_public_schema.api_orders
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
