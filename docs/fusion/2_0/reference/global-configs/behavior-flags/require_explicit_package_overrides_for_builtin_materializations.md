# Package overrides for built-in materializations

Removed in dbt Core v2

This flag was removed in dbt Core v2 and in Fusion. The new behavior is always enabled. If you're upgrading, remove this flag from your `dbt_project.yml`.

| require\_explicit\_package\_overrides\_for\_builtin\_materializations | dbt **Latest** | dbt Core       |
| --------------------------------------------------------------------- | -------------- | -------------- |
| Introduced                                                            | 2024.04        | 1.6.14, 1.7.14 |
| Matured (default → `true`)                                            | 2024.06        | 1.8.0          |
| Removed                                                               | —              | v2.0           |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Installed packages can no longer override built-in materializations without your explicit opt-in. A materialization defined in a package that matches the name of a built-in materialization is no longer included in the search and resolution order. Unlike macros, materializations don't use the `search_order` defined in the project `dispatch` config.

The built-in materializations are `'view'`, `'table'`, `'incremental'`, `'materialized_view'` for models, as well as `'test'`, `'unit'`, `'snapshot'`, `'seed'`, and `'clone'`.

You can still explicitly override built-in materializations by reimplementing the built-in materialization in your root project and wrapping the package implementation:

macros/materialization\_view\.sql

```sql
{% materialization view, snowflake %}
  {{ return(my_installed_package_name.materialization_view_snowflake()) }}
{% endmaterialization %}
```

In the future, we may extend the project-level [`dispatch` configuration](https://docs.getdbt.com/reference/project-configs/dispatch-config.md) to support a list of authorized packages for overriding built-in materializations.
