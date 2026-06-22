# Can I build my seeds in a schema other than my target schema or can I split my seeds across multiple schemas?

Yes! Use the [schema](https://docs.getdbt.com/reference/resource-configs/schema.md) configuration in your `dbt_project.yml` file.

dbt\_project.yml

```yml
name: jaffle_shop
...

seeds:
  jaffle_shop:
    +schema: mappings # all seeds in this project will use the schema "mappings" by default
    marketing:
      +schema: marketing # seeds in the "seeds/marketing/" subdirectory will use the schema "marketing"
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
