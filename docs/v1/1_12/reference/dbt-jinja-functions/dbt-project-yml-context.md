# About dbt\_project.yml context

The following context methods and variables are available when configuring resources in the `dbt_project.yml` file. This applies to the `models:`, `seeds:`, and `snapshots:` keys in the `dbt_project.yml` file.

**Available context methods:**

* [env\_var](./env_var.md)
* [var](./var.md) (Applies to dbt v1.12 and later) (*Note: Variables defined in `vars.yml` or with `--vars` are available*)

**Available context variables:**

* [target](./target.md)
* [builtins](./builtins.md)
* [dbt\_version](./dbt_version.md)

### Example configuration

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
