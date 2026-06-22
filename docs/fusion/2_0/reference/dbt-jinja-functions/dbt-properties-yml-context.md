# About properties.yml context

The following context methods and variables are available when configuring resources in a `properties.yml` file.

**Available context methods:**

* [env\_var](https://docs.getdbt.com/reference/dbt-jinja-functions/env_var.md)
* [var](https://docs.getdbt.com/reference/dbt-jinja-functions/var.md)

**Available context variables:**

* [target](https://docs.getdbt.com/reference/dbt-jinja-functions/target.md)
* [builtins](https://docs.getdbt.com/reference/dbt-jinja-functions/builtins.md)
* [dbt\_version](https://docs.getdbt.com/reference/dbt-jinja-functions/dbt_version.md)

### Example configuration[​](#example-configuration "Direct link to Example configuration")

properties.yml

```yml
# Configure this model to be materialized as a view
# in development and a table in production/CI contexts

models:
  - name: dim_customers
    config:
      materialized: "{{ 'view' if target.name == 'dev' else 'table' }}"
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
