💡Did you know\...

Available from dbt v

<!-- -->

1.12

<!-- -->

or with the

<!-- -->

[dbt "Latest" release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md).

# on\_error[Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

* Project file
* Property file
* SQL config

dbt\_project.yml

```yaml
models:
  <resource-path>:
    +on_error: skip_children | continue
```

models/properties.yml

```yaml
models:
  - name: [<model-name>]
    config:
      on_error: skip_children | continue
```

models/\<model\_name>.sql

```sql
{{ config(
    on_error="skip_children" | "continue"
) }}
```

## Definition[​](#definition "Direct link to Definition")

Beta feature

The `on_error` config is a beta feature in dbt Core v1.12.

The `on_error` config controls what happens to downstream (child) models when a model fails during a run. This config only applies to models; it has no effect on tests. To control whether downstream models run after a test failure, use the [`severity`](https://docs.getdbt.com/reference/resource-configs/severity.md) config on your tests instead.

`on_error` accepts two values:

* `skip_children` (default): All downstream models are skipped when the model fails.
* `continue`: Downstream models continue running when the model fails, instead of being skipped.

## Example[​](#example "Direct link to Example")

Set `on_error: continue` when downstream models can still run meaningfully even if an upstream model fails (for example, when they have fallback logic or independent data sources).

models/my\_model.sql

```sql
{{ config(
    materialized='table',
    on_error='continue'
) }}

select 1 as id
```

When `on_error` is set to `continue` on a model that fails, dbt runs its downstream models rather than skipping them. The failed model still appears as an error in the run results, and the overall run still fails even if all downstream models succeed.

The [`--fail-fast`](https://docs.getdbt.com/reference/global-configs/failing-fast.md) flag takes precedence over `on_error: continue`. When `--fail-fast` is set, dbt stops at the first failure and skips all remaining models, regardless of their `on_error` configuration.

## Behavior with multiple upstream models[​](#behavior-with-multiple-upstream-models "Direct link to Behavior with multiple upstream models")

When a model has multiple upstream models, `skip_children` takes precedence over `continue`. If any failed upstream model uses `skip_children`, the downstream model is skipped — even if other failed upstream models use `continue`.

For example, consider a DAG where `model_c` depends on both `model_a` and `model_b`:

* `model_a` uses `on_error: skip_children`
* `model_b` uses `on_error: continue`

The following table shows how `model_c` behaves based on the outcome of its upstream models:

| `model_a` result (`skip_children`) | `model_b` result (`continue`) | `model_c` behavior |
| ---------------------------------- | ----------------------------- | ------------------ |
| Success                            | Success                       | Runs               |
| Error                              | Success                       | Skipped            |
| Success                            | Error                         | Runs               |
| Error                              | Error                         | Skipped            |

<br />

When `model_a` errors, `model_c` is always skipped because `model_a` uses `skip_children`. When only `model_b` errors, `model_c` still runs because `model_b` uses `continue`, which does not block downstream models.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
