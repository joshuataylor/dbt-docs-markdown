# No spaces in source and semantic model names

| require\_source\_and\_semantic\_model\_names\_without\_spaces | dbt **Latest** | dbt Core |
| ------------------------------------------------------------- | -------------- | -------- |
| Introduced                                                    | 2026.4         | 1.12.0   |
| Matured (default → `true`)                                    | —              | —        |
| Removed                                                       | —              | —        |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

The `require_source_and_semantic_model_names_without_spaces` flag is set to `false` by default.

Source names and semantic model names should contain letters, numbers, and underscores — *not* spaces. dbt raises the [`ResourceNamesWithSpacesDeprecation`](https://docs.getdbt.com/reference/deprecations.md#resourcenameswithspacesdeprecation) warning if it detects a space in a source name or semantic model name. When the `require_source_and_semantic_model_names_without_spaces` flag is set to `true`, dbt raises an error.

This flag extends [`require_resource_names_without_spaces`](https://docs.getdbt.com/reference/global-configs/behavior-flags/require_resource_names_without_spaces.md) to cover source and semantic model names specifically.
