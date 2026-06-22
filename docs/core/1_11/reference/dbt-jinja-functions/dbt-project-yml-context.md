# About dbt\_project.yml context

The following context methods and variables are available when configuring resources in the `dbt_project.yml` file. This applies to the `models:`, `seeds:`, and `snapshots:` keys in the `dbt_project.yml` file.

**Available context methods:**

* [env\_var](https://docs.getdbt.com/reference/dbt-jinja-functions/env_var.md)
* [var](https://docs.getdbt.com/reference/dbt-jinja-functions/var.md)
  <!-- -->
  (*Note: Only variables defined with `--vars` are available*)

**Available context variables:**

* [target](https://docs.getdbt.com/reference/dbt-jinja-functions/target.md)
* [builtins](https://docs.getdbt.com/reference/dbt-jinja-functions/builtins.md)
* [dbt\_version](https://docs.getdbt.com/reference/dbt-jinja-functions/dbt_version.md)

### Example configuration[​](#example-configuration "Direct link to Example configuration")

dbt\_project.yml

```yml
name: my_project
version: 1.0.0

# Configure the models in models/facts/ to be materialized as views
# in development and tables in production/CI contexts

models:
  my_project:
    facts:
      +materialized: "{{ 'view' if target.name == 'dev' else 'table' }}"
```
