# lag\_tolerance

* Project YAML file
* Properties YAML file
* SQL file config

dbt\_project.yml

```yaml
models:
  <resource-path>:
    +state:
      lag_tolerance: <duration_string>
```

models/\<filename>.yml

```yaml
models:
  - name: my_model
    config:
      state:
        lag_tolerance: <duration_string>
```

models/\<filename>.sql

```sql
{{ config(
    state={
      "lag_tolerance": "<duration_string>"
    }
) }}
```

## Definition[​](#definition "Direct link to Definition")

Source systems may update more frequently than downstream models need to rebuild. For example, a model used for daily reporting doesn't need to refresh more than once per day, even if new upstream data is available hourly.

With `lag_tolerance`, you can prevent those unnecessary rebuilds. When dbt State evaluates whether to rebuild a node, it checks whether upstream parents have fresh data that exceeds the `lag_tolerance` threshold. If they haven't, dbt reuses the existing node rather than cloning or rebuilding it.

This config accepts two value types:

* **Duration strings** in the format `<number><unit>`:

  | Unit    | Accepted values          |
  | ------- | ------------------------ |
  | Seconds | `s`, `second`, `seconds` |
  | Minutes | `m`, `minute`, `minutes` |
  | Hours   | `h`, `hour`, `hours`     |
  | Days    | `d`, `day`, `days`       |
  | Weeks   | `w`, `week`, `weeks`     |

* **Jinja expressions** - `lag_tolerance` is evaluated as a Jinja template, so you can use any dbt context variables (`target`, `var()`, `env_var()`) to set dynamic tolerances. This is useful for applying different tolerances per environment without duplicating config blocks:

  ```yaml
  lag_tolerance: "{{ '4h' if target.name == 'prod' else '7d' }}"
  ```

### When does `lag_tolerance` apply[​](#when-does-lag_tolerance-apply "Direct link to when-does-lag_tolerance-apply")

`lag_tolerance` only applies to data freshness checks. A downstream model still rebuilds within its tolerance window if an upstream model's compiled SQL has changed since the last run, regardless of the `lag_tolerance` setting.

This often happens with incremental models. The first time an incremental model runs, it executes a full load with no `WHERE` clause. On subsequent runs, `is_incremental()` becomes true and a filter is appended, changing the compiled SQL. dbt State detects this as a query change on the upstream model and rebuilds all downstream models, even those whose `lag_tolerance` has not elapsed.

For example, `fct_orders` is an incremental model that `agg_orders_daily` depends on:

models/fct\_orders.sql

```sql
{{ config(materialized='incremental', unique_key='id') }}

select id, amount from {{ ref('raw_orders') }}
{% if is_incremental() %}
where id > (select max(id) from {{ this }})
{% endif %}
```

models/agg\_orders\_daily.sql

```sql
{{ config(materialized='table', state={'lag_tolerance': '3h'}) }}

select date_trunc('day', created_at) as day, sum(amount) as total
from {{ ref('fct_orders') }}
group by 1
```

When `fct_orders` transitions from a full load to an incremental run, its compiled SQL changes. `agg_orders_daily` rebuilds on that run despite its 3-hour `lag_tolerance`.

## Default[​](#default "Direct link to Default")

`45m`. When `lag_tolerance` is not set, dbt State applies a default tolerance of 45 minutes.

## Examples[​](#examples "Direct link to Examples")

### Use different tolerances per environment[​](#use-different-tolerances-per-environment "Direct link to Use different tolerances per environment")

Use a Jinja expression to set a tight tolerance in production and a looser one everywhere else. This keeps production data fresh while reducing unnecessary rebuilds during development:

dbt\_project.yml

```yaml
models:
  +state:
    lag_tolerance: "{{ '4h' if target.name == 'prod' else '7d' }}"
```

In this example, models in the `prod` target rebuild only when upstream data is more than 4 hours old. In all other environments, models wait 7 days before rebuilding.

### Apply different tolerances per folder[​](#apply-different-tolerances-per-folder "Direct link to Apply different tolerances per folder")

Set different tolerances for different parts of your project by targeting folders:

dbt\_project.yml

```yaml
models:
  <your_project>:
    marts:
      +state:
        lag_tolerance: 1d
    staging:
      +state:
        lag_tolerance: 1h
```

### Override for a specific model[​](#override-for-a-specific-model "Direct link to Override for a specific model")

Override the project-level default for a single model:

models/my\_model.yml

```yaml
models:
  - name: my_model
    config:
      state:
        lag_tolerance: 1h
```

## Related docs[​](#related-docs "Direct link to Related docs")

* [About dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md)
* [Set up dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-setup.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
