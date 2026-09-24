# lag\_tolerance

## Project YAML file

dbt\_project.yml

```yaml
models:
  <resource-path>:
    +state:
      lag_tolerance: <duration_string>
```

Report incorrect code

## Properties YAML file

models/\<filename>.yml

```yaml
models:
  - name: my_model
    config:
      state:
        lag_tolerance: <duration_string>
```

Report incorrect code

## SQL file config

models/\<filename>.sql

```sql
{{ config(
    state={
      "lag_tolerance": "<duration_string>"
    }
) }}
```

Report incorrect code

## Definition

Source systems may update more frequently than some downstream models need to rebuild. For example, a model used for daily reporting doesn't need to refresh more than once per day, even if new upstream data is available hourly, while the model powering customer facing metrics that uses some of the same sources may need to update every 30 minutes.

`lag_tolerance` sets how long dbt State waits before rebuilding a node once its upstream data changes. A node rebuilds only when *both* are true: its last build is older than the `lag_tolerance` window, and its upstream data has changed since that build. This acts as a compute-saving buffer that helps you stay aligned with data freshness [Service Level Agreements (SLAs)](https://www.getdbt.com/blog/data-slas-best-practices) without unnecessary rebuilds. It supports two key scenarios:

* **Aligning builds with SLA requirements**: `lag_tolerance` allows you to align model execution directly with data freshness SLA requirements, decoupling high-frequency upstream changes from downstream models that operate under wider, less demanding freshness requirements.
* **Protecting compute during upstream SLA breaches**: `lag_tolerance` protects your compute budget during freshness SLA breaches, preventing costly downstream rebuilds on static data when an upstream dependency fails its freshness SLA.

The `lag_tolerance` config accepts two value types:

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

  Report incorrect code

### When a node rebuilds

dbt State rebuilds a node only when *both* conditions are met. If either is false, it reuses the existing node:

| Condition                 | What it means                                                     |
| ------------------------- | ----------------------------------------------------------------- |
| The tolerance has elapsed | The time since the node's last build exceeds its `lag_tolerance`. |
| Upstream data has changed | At least one upstream dependency has new data since that build.   |

`lag_tolerance` sets a minimum time between rebuilds

`lag_tolerance` controls how often a node can rebuild, not how fresh its upstream data has to be.

#### Example

Let's say you have a job that runs every 30 minutes, your model has a `45m` tolerance, and it last built at `08:00`. New upstream data arrived at `08:20`.

| Job run | Time since last build | New upstream data since last build? | Result                                                            |
| ------- | --------------------- | ----------------------------------- | ----------------------------------------------------------------- |
| `08:30` | `30m`                 | Yes                                 | Reuse: tolerance hasn't elapsed.                                  |
| `09:00` | `60m`                 | Yes                                 | Rebuild: both conditions are met.                                 |
| `09:30` | `30m`                 | No                                  | Reuse: tolerance hasn't elapsed and upstream data hasn't changed. |
| `10:00` | `60m`                 | No                                  | Reuse: upstream data hasn't changed since `09:00`.                |

The `08:20` data waited until `09:00` to be picked up. This was the first run where the build was older than `45m` *and* upstream data had changed. At `10:00`, the build was old enough, but because no new upstream data had arrived since `09:00`, dbt reused the node.

To rebuild a node whenever its upstream data changes, set `lag_tolerance` to `0s`:

```yaml
state:
  lag_tolerance: 0s
```

Report incorrect code

### When does `lag_tolerance` apply

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

Report incorrect code

models/agg\_orders\_daily.sql

```sql
{{ config(materialized='table', state={'lag_tolerance': '3h'}) }}

select date_trunc('day', created_at) as day, sum(amount) as total
from {{ ref('fct_orders') }}
group by 1
```

Report incorrect code

When `fct_orders` transitions from a full load to an incremental run, its compiled SQL changes. `agg_orders_daily` rebuilds on that run despite its 3-hour `lag_tolerance`.

tip

To help you tune `lag_tolerance` values, the **dbt State** page on the dbt platform provides [lag tolerance recommendations](../../docs/deploy/dbt-state-interface.md#lag-tolerance-recommendations) based on your models' 30-day build history, so you can see which models would benefit from a higher tolerance.

## Default

If you don't set `lag_tolerance`, dbt State uses `45m` (45 minutes).

## Examples

### Use different tolerances per environment

Use a Jinja expression to set a shorter tolerance in production and a longer tolerance elsewhere. This keeps production data fresh while reducing unnecessary rebuilds during development:

dbt\_project.yml

```yaml
models:
  +state:
    lag_tolerance: "{{ '4h' if target.name == 'prod' else '7d' }}"
```

Report incorrect code

In this example, models in the `prod` target rebuild once their last build is more than 4 hours old and their upstream data has changed. In all other environments, models rebuild once their last build is more than 7 days old and their upstream data has changed.

### Vary tolerance by day of the week

Use a Jinja expression to evaluate the day of the week and apply a tighter tolerance on weekdays than on weekends:

dbt\_project.yml

```yaml
models:
  +state:
    lag_tolerance: "{{ '24h' if modules.datetime.datetime.today().weekday() in (5, 6) else '1h' }}"
```

Report incorrect code

In this example, models rebuild once their last build is more than 1 hour old (Monday to Friday) or more than 24 hours old (Saturday to Sunday) and their upstream data has changed.

### Apply different tolerances per folder

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

Report incorrect code

### Override for a specific model

Override the project-level default for a single model:

models/my\_model.yml

```yaml
models:
  - name: my_model
    config:
      state:
        lag_tolerance: 1h
```

Report incorrect code

## Related docs

* [About dbt State](../../docs/deploy/dbt-state-about.md)
* [Set up dbt State](../../docs/deploy/dbt-state-setup.md)
* [Monitor dbt State activity](../../docs/deploy/dbt-state-interface.md)
