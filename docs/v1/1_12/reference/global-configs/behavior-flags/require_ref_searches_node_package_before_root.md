# Package ref search order

| require\_ref\_searches\_node\_package\_before\_root | dbt **Latest** | dbt Core |
| --------------------------------------------------- | -------------- | -------- |
| Introduced                                          | 2025.12        | 1.11.0   |
| Matured (default → `true`)                          | —              | —        |
| Removed                                             | —              | —        |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

The `require_ref_searches_node_package_before_root` flag controls the search order when dbt resolves `ref()` calls defined within a package.

The flag is set to `false` by default in **Latest** and dbt Core v1.11. When dbt resolves a `ref()` in a package model, it searches for the referenced model in the root project *first*, then in the package where the model is defined.

For example, the following model in the package `my_package` is imported by the project `my_project`:

my\_package/model\_downstream.sql

```sql
select * from {{ ref('model_upstream') }}
```

By default, dbt searches for `model_upstream` in this order:

1. First in `my_project` (root project)
2. Then in `my_package` (where the model is defined)

When you set the `require_ref_searches_node_package_before_root` flag to `true`, dbt searches the package where the model is defined *before* searching the root project.

Using the same example, dbt searches for `model_upstream` in this order:

1. First in `my_package` (where the model is defined)
2. Then in `my_project` (root project)

The current default behavior is considered a [bug in dbt-core](https://github.com/dbt-labs/dbt-core/issues/11351) because it can potentially lead to unexpected dependency cycles. However, because this is long-standing behavior, changing the default requires setting `require_ref_searches_node_package_before_root` to `true` to avoid breaking existing projects.
