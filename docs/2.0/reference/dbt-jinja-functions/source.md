# About source function

```sql
select * from {{ source("source_name", "table_name") }}
```

## Definition[​](#definition "Direct link to Definition")

This function:

* Returns a [Relation](https://docs.getdbt.com/reference/dbt-classes.md#relation) for a [source](https://docs.getdbt.com/docs/build/sources.md)
* Creates dependencies between a source and the current model, which is useful for documentation and [node selection](https://docs.getdbt.com/reference/node-selection/syntax.md)
* Compiles to the full object name in the database

## Related guides[​](#related-guides "Direct link to Related guides")

* [Using sources](https://docs.getdbt.com/docs/build/sources.md)

## Arguments[​](#arguments "Direct link to Arguments")

* `source_name`: The `name:` defined under a `sources:` key
* `table_name`: The `name:` defined under a `tables:` key

## Example[​](#example "Direct link to Example")

Consider a source defined as follows:

models/\<filename>.yml

```yaml

sources:
  - name: jaffle_shop # this is the source_name
    database: raw

    tables:
      - name: customers # this is the table_name
      - name: orders
```

Select from the source in a model:

models/orders.sql

```sql
select
  ...

from {{ source('jaffle_shop', 'customers') }}

left join {{ source('jaffle_shop', 'orders') }} using (customer_id)
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
