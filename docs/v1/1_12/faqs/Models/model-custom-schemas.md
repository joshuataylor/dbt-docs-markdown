# Can I build my models in a schema other than my target schema or split my models across multiple schemas?

Yes! Use the [schema](https://docs.getdbt.com/reference/resource-configs/schema.md) configuration in your `dbt_project.yml` file, or using a `config` block:

dbt\_project.yml

```yml
name: jaffle_shop
...

models:
  jaffle_shop:
    marketing:
      +schema: marketing # models in the `models/marketing/` subdirectory will use the marketing schema
```

models/customers.sql

```sql
{{
  config(
    schema='core'
  )
}}
```
