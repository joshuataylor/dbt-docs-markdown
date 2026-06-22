# dbt State configurations

When you run a dbt command with [dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md) enabled, dbt compares a node's logic and data against previous builds and takes the most efficient path:

* **Reuse** — if the object exists in the target schema, its logic hasn't changed, and its parents haven't received fresh data exceeding its `lag_tolerance`, the node is reused.
* **Clone** — if reuse isn't possible but the object exists in the deferred environment with the same logic and sufficiently fresh data, dbt State clones it.
* **Normal build** — if neither reuse nor clone is possible, the node builds as normal, using deferral for any unselected upstream nodes.

Use the following configs to control how dbt State makes these decisions.

* Project YAML file
* Properties YAML file
* SQL file config

dbt\_project.yml

```yaml
models:
  +state:
    lag_tolerance: <duration>
    require_fresh_data_from: any | all
    evaluate_volatile_sql: true | false
    pre_clone: never | if_missing | always
    execute_hooks_on_any_reuse: true | false
```

models/schema.yml

```yaml
models:
  - name: <model_name>
    config:
      state:
        lag_tolerance: <duration>
        require_fresh_data_from: any | all
        evaluate_volatile_sql: true | false
        pre_clone: never | if_missing | always
        execute_hooks_on_any_reuse: true | false
```

models/\<filename>.sql

```sql
{{ config(
    state={
        "lag_tolerance": "<duration>",
        "require_fresh_data_from": "any" | "all",
        "evaluate_volatile_sql": true | false,
        "pre_clone": "never" | "if_missing" | "always",
        "execute_hooks_on_any_reuse": true | false
    }
) }}
```

| Config                                                                                                           | Default             | Scope                                           | Description                                                                                                                                                                                                                                        |
| ---------------------------------------------------------------------------------------------------------------- | ------------------- | ----------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`lag_tolerance`](https://docs.getdbt.com/reference/resource-configs/lag-tolerance.md)                           | `45m`               | Node, folder, or project-level via model config | How much time must pass since the last upstream data change before a node is eligible for a rebuild. Applies to data freshness only; if an upstream model's compiled SQL has changed, the node rebuilds regardless of the `lag_tolerance` setting. |
| [`require_fresh_data_from`](https://docs.getdbt.com/reference/resource-configs/require-fresh-data-from.md)       | `any`               | Node, folder, or project-level via model config | Whether `any` or `all` direct parents need fresh data before a node is eligible for a rebuild.                                                                                                                                                     |
| [`pre_clone`](https://docs.getdbt.com/reference/resource-configs/pre-clone.md)                                   | `if_missing`        | Node, folder, or project-level via model config | Whether dbt State pre-populates incremental models and snapshots by cloning production before a run.                                                                                                                                               |
| [`execute_hooks_on_any_reuse`](https://docs.getdbt.com/reference/resource-configs/execute-hooks-on-any-reuse.md) | `false`             | Node, folder, or project-level via model config | Whether pre- and post-hooks run when a node is reused without rebuilding.                                                                                                                                                                          |
| [`evaluate_volatile_sql`](https://docs.getdbt.com/reference/resource-configs/evaluate-volatile-sql.md)           | `false`             | Node, folder, or project-level via model config | Whether dbt State stores and compares the runtime output of volatile SQL functions when deciding whether to rebuild.                                                                                                                               |
| [`defer_to_target`](https://docs.getdbt.com/reference/resource-configs/defer-to-target.md)                       | `prod`              | Profile                                         | (Self-managed only) Which profile target dbt State defers to.                                                                                                                                                                                      |
| [`metadata_warehouse`](https://docs.getdbt.com/reference/resource-configs/metadata-warehouse.md)                 | Profile `warehouse` | Profile                                         | (Snowflake only) A separate warehouse for dbt State metadata lookups.                                                                                                                                                                              |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Related docs[​](#related-docs "Direct link to Related docs")

* [About dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md)
* [Set up dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-setup.md)
