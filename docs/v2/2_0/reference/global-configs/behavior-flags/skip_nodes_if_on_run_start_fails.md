# Failures in on-run-start hooks

Removed in dbt Core 2.0

This flag was removed in dbt Core 2.0 and in Fusion. The new behavior is always enabled. If you're upgrading, remove this flag from your `dbt_project.yml`.

| skip\_nodes\_if\_on\_run\_start\_fails | dbt **Latest** | dbt Core |
| -------------------------------------- | -------------- | -------- |
| Introduced                             | 2024.10        | 1.9.0    |
| Matured (default → `true`)             | Sep 1, 2026    | 1.12.0   |
| Removed                                | —              | v2.0     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

<br />

Starting in dbt Core v1.12, `skip_nodes_if_on_run_start_fails` defaults to `true`, skipping all selected resources if there is a failure on an `on-run-start` hook.

For more information, refer to [`on-run-start` / `on-run-end`](https://docs.getdbt.com/reference/project-configs/on-run-start-on-run-end.md).

## Impact[​](#impact "Direct link to Impact")

If your project uses `on-run-start` hooks for non-critical work (for example, telemetry, notifications, audit inserts, attaching session settings), your build will stop producing output whenever a hook fails. Tables and views that previously refreshed daily will stop updating the next time the hook fails.

 Recommended actions

If every model is skipped after an `on-run-start` hook fails, find which `on-run-start` hook failed in the run log — it appears as an `ERROR` immediately before all the nodes marked `SKIP`. From there, either remove the hook if it's not required, or fix the root cause of the failure by reviewing the hook's SQL statements.

To opt out of this behavior, set the flag to `false`:

dbt\_project.yml

```yaml
flags:
  skip_nodes_if_on_run_start_fails: false
```
