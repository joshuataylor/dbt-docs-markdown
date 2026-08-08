# About properties.yml context

The following context methods and variables are available when configuring resources in a `properties.yml` file.

**Available context methods:**

* [env\_var](./env_var.md)
* [var](./var.md)

**Available context variables:**

* [target](./target.md)
* [builtins](./builtins.md)
* [dbt\_version](./dbt_version.md)

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
