# Project hooks with source freshness

Removed in dbt Core 2.0

This flag was removed in dbt Core 2.0 and in Fusion. The new behavior is always enabled. If you're upgrading, remove this flag from your `dbt_project.yml`.

| source\_freshness\_run\_project\_hooks | dbt **Latest** | dbt Core |
| -------------------------------------- | -------------- | -------- |
| Introduced                             | 2024.03        | 1.8.0    |
| Matured (default → `true`)             | 2025.05        | 1.10.0   |
| Removed                                | —              | v2.0     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Project hooks ([`on-run-start` / `on-run-end`](https://docs.getdbt.com/reference/project-configs/on-run-start-on-run-end.md)) now run as part of the `dbt source freshness` command by default. Previously, hooks did not execute during `dbt source freshness`.

If you have hooks that should not run before or after `dbt source freshness`, add a conditional check:

dbt\_project.yml

```yaml
on-run-start:
  - "{{ ... if flags.WHICH != 'freshness' }}"
```
