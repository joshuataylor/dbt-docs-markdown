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

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
