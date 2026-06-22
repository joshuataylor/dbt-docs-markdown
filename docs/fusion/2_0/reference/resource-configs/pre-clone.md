# pre\_clone

* Project YAML file
* Properties YAML file
* SQL file config

dbt\_project.yml

```yaml
models:
  <resource-path>:
    +state:
      pre_clone: never | if_missing | always
```

models/\<filename>.yml

```yaml
models:
  - name: my_model
    config:
      state:
        pre_clone: never | if_missing | always
```

models/\<filename>.sql

```sql
{{ config(
    state={
      "pre_clone": "never" | "if_missing" | "always"
    }
) }}
```

## Definition[​](#definition "Direct link to Definition")

`pre_clone` controls whether and when dbt State pre-populates incremental models and snapshots in your development environment by cloning their production counterparts before a run.

| Value                  | Behavior                                                                                                                                                           |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `never`                | Never pre-clone. Incremental models and snapshots start from whatever exists in the development environment. If it doesn't already exist, it's built from scratch. |
| `if_missing` (default) | Clone only if the target table does not yet exist in development. After the first clone, subsequent runs build incrementally on top of the cloned data.            |
| `always`               | Clone before every run, regardless of whether the development table already exists. Guarantees development incremental state matches production on every run.      |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

note

Non-incremental materializations are never pre-cloned. This setting has no effect on them.

## Examples[​](#examples "Direct link to Examples")

### Always sync development with production[​](#always-sync-development-with-production "Direct link to Always sync development with production")

Use `always` when you want development to reflect the current production state before every run:

models/fct\_orders.yml

```yaml
models:
  - name: fct_orders
    config:
      state:
        pre_clone: always
```

### Isolate dev state from production[​](#isolate-dev-state-from-production "Direct link to Isolate dev state from production")

Use `never` when you want development to be fully independent from production:

dbt\_project.yml

```yaml
models:
  +state:
    pre_clone: never
```

## Related docs[​](#related-docs "Direct link to Related docs")

* [About dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md)
* [Set up dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-setup.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
