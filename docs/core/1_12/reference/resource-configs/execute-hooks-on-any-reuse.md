# execute\_hooks\_on\_any\_reuse

* Project YAML file
* Properties YAML file
* SQL file config

dbt\_project.yml

```yaml
models:
  <resource-path>:
    +state:
      execute_hooks_on_any_reuse: true | false
```

models/\<filename>.yml

```yaml
models:
  - name: my_model
    config:
      state:
        execute_hooks_on_any_reuse: true | false
```

models/\<filename>.sql

```sql
{{ config(
    state={
      "execute_hooks_on_any_reuse": true | false
    }
) }}
```

## Definition[​](#definition "Direct link to Definition")

When dbt State skips a node because it's still fresh, that node's pre- and post-hooks are not executed by default. This matches dbt's standard behavior: if the node wasn't executed, its hooks don't run.

Set `execute_hooks_on_any_reuse: true` if you have audit hooks or other hooks that must run on every job invocation, regardless of whether the node was rebuilt or reused.

| Value             | Behavior                                                    |
| ----------------- | ----------------------------------------------------------- |
| `false` (default) | Hooks are skipped when a node is reused without rebuilding. |
| `true`            | Hooks run even when a node is reused.                       |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Hooks always execute when dbt State *clones* a node from another environment, because the clone step creates a new object in the warehouse.

## Example[​](#example "Direct link to Example")

### Always run audit hooks[​](#always-run-audit-hooks "Direct link to Always run audit hooks")

Use `execute_hooks_on_any_reuse: true` for nodes with audit or lineage hooks that must run on every job invocation:

models/fct\_orders.yml

```yaml
models:
  - name: fct_orders
    config:
      state:
        execute_hooks_on_any_reuse: true
      post-hook:
        - "INSERT INTO audit_log VALUES ('{{ model.name }}', CURRENT_TIMESTAMP())"
```

## Related docs[​](#related-docs "Direct link to Related docs")

* [About dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md)
* [Set up dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-setup.md)
* [Hooks](https://docs.getdbt.com/docs/build/hooks-operations.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
