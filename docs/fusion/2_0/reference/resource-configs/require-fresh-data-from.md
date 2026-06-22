# require\_fresh\_data\_from

* Project YAML file
* Properties YAML file
* SQL file config

dbt\_project.yml

```yaml
models:
  <resource-path>:
    +state:
      require_fresh_data_from: any | all
```

models/\<filename>.yml

```yaml
models:
  - name: my_model
    config:
      state:
        require_fresh_data_from: any | all
```

models/\<filename>.sql

```sql
{{ config(
    state={
      "require_fresh_data_from": "any" | "all"
    }
) }}
```

## Definition[​](#definition "Direct link to Definition")

`require_fresh_data_from` controls how many direct parent nodes need fresh data before dbt State considers this node eligible for a rebuild. dbt State still looks for an object to reuse before triggering an actual rebuild.

| Value           | Behavior                                                                                                                         |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| `any` (default) | The node becomes eligible for a rebuild when *any* direct parent has fresh data. Rebuilds more frequently; may increase spend.   |
| `all`           | The node only becomes eligible for a rebuild when *all* direct parents have fresh data. Rebuilds less frequently; reduces spend. |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Default[​](#default "Direct link to Default")

`any`

## Examples[​](#examples "Direct link to Examples")

### Require all parents to have fresh data[​](#require-all-parents-to-have-fresh-data "Direct link to Require all parents to have fresh data")

Use `all` for a model that joins multiple sources and should only rebuild when every upstream dependency has updated:

models/fct\_orders.yml

```yaml
models:
  - name: fct_orders
    config:
      state:
        require_fresh_data_from: all
```

### Apply a project-wide default[​](#apply-a-project-wide-default "Direct link to Apply a project-wide default")

dbt\_project.yml

```yaml
models:
  +state:
    require_fresh_data_from: all
```

## Related docs[​](#related-docs "Direct link to Related docs")

* [About dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md)
* [Set up dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-setup.md)
