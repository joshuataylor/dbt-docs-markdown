# No spaces in resource names

Removed in dbt Core v2

This flag was removed in dbt Core v2 and in Fusion. The new behavior is always enabled. If you're upgrading, remove this flag from your `dbt_project.yml`.

| require\_resource\_names\_without\_spaces | dbt **Latest** | dbt Core |
| ----------------------------------------- | -------------- | -------- |
| Introduced                                | 2024.05        | 1.8.0    |
| Matured (default → `true`)                | 2025.05        | 1.10.0   |
| Removed                                   | —              | v2.0     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

dbt raises an error if it detects a space in a resource name. Resource names should contain letters, numbers, and underscores only.

dbt raises the [`ResourceNamesWithSpacesDeprecation`](https://docs.getdbt.com/reference/deprecations.md#resourcenameswithspacesdeprecation) warning if it detects a space in a resource name. When this flag is set to `true` (now always-on in dbt Core v2), dbt raises an error instead.

models/model name with spaces.sql

```sql
-- This model file should be renamed to model_name_with_underscores.sql
```

See also [`require_source_and_semantic_model_names_without_spaces`](https://docs.getdbt.com/reference/global-configs/behavior-flags/require_source_and_semantic_model_names_without_spaces.md), which extends this behavior to source and semantic model names.
