# About profiles.yml context

The following context methods are available when configuring resources in the `profiles.yml` file.

**Available context methods:**

* [env\_var](https://docs.getdbt.com/reference/dbt-jinja-functions/env_var.md)
* [var](https://docs.getdbt.com/reference/dbt-jinja-functions/var.md) (*Note: only variables defined with `--vars` are available*)

### Example usage[​](#example-usage "Direct link to Example usage")

\~/.dbt/profiles.yml

```yml
jaffle_shop:
  target: dev
  outputs:
    dev:
      type: redshift
      host: "{{ env_var('DBT_HOST') }}"
      user: "{{ env_var('DBT_USER') }}"
      password: "{{ env_var('DBT_PASS') }}"
      port: 5439
      dbname: analytics
      schema: dbt_dbanin
      threads: 4
```
