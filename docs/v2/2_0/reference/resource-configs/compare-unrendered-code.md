# compare\_unrendered\_code

## Project YAML file

dbt\_project.yml

```yaml
models:
  <resource-path>:
    +state:
      compare_unrendered_code: true | false
```

## Properties YAML file

models/\<filename>.yml

```yaml
models:
  - name: my_model
    config:
      state:
        compare_unrendered_code: true | false
```

## SQL file config

models/\<filename>.sql

```sql
{{ config(
    state={
      "compare_unrendered_code": true | false
    }
) }}
```

## Definition

| Value             | Behavior                                                                                                                                              |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `false` (default) | dbt State compares only rendered SQL. Non-deterministic macros or env vars that change the rendered SQL on each run may trigger unnecessary rebuilds. |
| `true`            | dbt State avoids unnecessary rebuilds by checking both the Jinja template and rendered SQL. A rebuild only occurs when both have changed.             |

By default, dbt State compares only rendered SQL and rebuilds when the output differs from the stored hash. How frequently that happens depends on how often the values change. For example:

* `{{ invocation_id() }}` resolves to a new value on every run, so it always triggers a rebuild.
* `{{ dbt_utils.get_column_values() }}` only changes if the underlying column values change or return in a different order, so it triggers rebuilds less predictably.
* `{{ modules.datetime.datetime.now().month }}` only resolves to a new value once a month, so it triggers a rebuild on the next run after the month changes.

Set `compare_unrendered_code: true` to avoid these unnecessary rebuilds. When enabled, dbt State also checks the unrendered Jinja template, and only rebuilds when both the unrendered code *and* the rendered SQL have changed. This is useful for nodes with non-deterministic macros or environment variables (such as a macro that returns a UUID, or `{{ env_var('AIRFLOW_RUN_ID') }}`) that produce different rendered SQL on every run even when the template itself is unchanged.

## Interaction with `evaluate_volatile_sql`

When `compare_unrendered_code` is enabled, dbt State checks whether the unrendered Jinja template has changed. If the template is unchanged, dbt State skips SQL parsing entirely — volatile SQL functions are not evaluated and [`evaluate_volatile_sql`](./evaluate-volatile-sql.md) has no effect. If the unrendered template has changed, dbt State proceeds to parse and compare the rendered SQL, and `evaluate_volatile_sql` applies.

## Examples

### Model with a non-deterministic environment variable

A model with a non-deterministic environment variable is reused as long as neither the model's own Jinja template nor the source of any macros it calls has changed, even if the environment variable (for example, AIRFLOW\_RUN\_ID) resolves to a different value at runtime:

models/fct\_events.sql

```sql
{{ config(state={"compare_unrendered_code": true}) }}

select
  event_id,
  user_id,
  event_type,
  occurred_at,
  '{{ env_var("AIRFLOW_RUN_ID") }}' as airflow_run_id
from {{ ref('stg_events') }}
```

### Model with dbt run metadata columns

A model that records which dbt run produced each row using [`invocation_id()`](../dbt-jinja-functions/invocation_id.md) or [`run_started_at`](../dbt-jinja-functions/run_started_at.md) is reused as long as the template hasn't changed, even though those values resolve differently on every run:

models/fct\_orders.sql

```sql
{{ config(state={"compare_unrendered_code": true}) }}

select
  *,
  '{{ invocation_id() }}' as dbt_invocation_id,
  '{{ run_started_at }}' as dbt_run_started_at
from {{ ref('stg_orders') }}
```

## Related docs

* [About dbt State](../../docs/deploy/dbt-state-about.md)
* [dbt State configs](./dbt-state-configs.md)
* [Set up dbt State](../../docs/deploy/dbt-state-setup.md)
