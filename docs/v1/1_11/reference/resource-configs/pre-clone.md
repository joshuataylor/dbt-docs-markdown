# pre\_clone

## Project YAML file

dbt\_project.yml

```yaml
models:
  <resource-path>:
    +state:
      pre_clone: never | if_missing | always
```

## Properties YAML file

models/\<filename>.yml

```yaml
models:
  - name: my_model
    config:
      state:
        pre_clone: never | if_missing | always
```

## SQL file config

models/\<filename>.sql

```sql
{{ config(
    state={
      "pre_clone": "never" | "if_missing" | "always"
    }
) }}
```

## Definition

`pre_clone` controls whether and when dbt State pre-populates incremental models and snapshots in your development environment by cloning their production counterparts before a run.

| Value                  | Behavior                                                                                                                                                           |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `never`                | Never pre-clone. Incremental models and snapshots start from whatever exists in the development environment. If it doesn't already exist, it's built from scratch. |
| `if_missing` (default) | Clone only if the target table does not yet exist in development. After the first clone, subsequent runs build incrementally on top of the cloned data.            |
| `always`               | Clone before every run, regardless of whether the development table already exists. Guarantees development incremental state matches production on every run.      |

note

Non-incremental materializations are never pre-cloned. This setting has no effect on them.

## Examples

### Always sync development with production

Use `always` when you want development to reflect the current production state before every run:

models/fct\_orders.yml

```yaml
models:
  - name: fct_orders
    config:
      state:
        pre_clone: always
```

### Isolate dev state from production

Use `never` when you want development to be fully independent from production:

dbt\_project.yml

```yaml
models:
  +state:
    pre_clone: never
```

## Related docs

* [About dbt State](../../docs/deploy/dbt-state-about.md)
* [Set up dbt State](../../docs/deploy/dbt-state-setup.md)
