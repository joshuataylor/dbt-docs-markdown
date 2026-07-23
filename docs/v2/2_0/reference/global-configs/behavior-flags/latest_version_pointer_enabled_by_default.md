# Latest version pointer for versioned models

| latest\_version\_pointer\_enabled\_by\_default | dbt **Latest** | dbt Core | Fusion         |
| ---------------------------------------------- | -------------- | -------- | -------------- |
| Introduced                                     | 2026.5         | 1.12.0   | Early preview  |
| Matured (default → `true`)                     | —              | —        | Already `true` |
| Removed                                        | —              | —        | —              |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

The `latest_version_pointer_enabled_by_default` flag is set to `false` by default in dbt Core v1. In Fusion, this flag defaults to `true`, which enables the latest version pointer for all versioned models automatically.

When you set it to `true`, dbt automatically creates a [latest version pointer](https://docs.getdbt.com/docs/mesh/govern/model-versions.md#pointing-to-the-latest-version) view for every versioned model in the project, without requiring per-model configuration. The pointer view is named after the model's base name (for example, `dim_customers`) and always points to the relation for the model with `is_latest_version: true` (for example, `dim_customers_v2`).

Without this flag, you must opt in per model by setting [`latest_version_pointer.enabled: true`](https://docs.getdbt.com/reference/resource-configs/latest_version_pointer.md) in the model config.
