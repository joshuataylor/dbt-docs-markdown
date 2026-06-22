# Migrating from state-aware orchestration to dbt State [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

dbt State improves upon state-aware orchestration in a few key ways:

* **Works everywhere** — dbt State works with dbt Core, Fusion, and dbt platform, as well as external orchestrators, across both development and deployment environments.
* **Smarter data freshness tracking** — dbt State tracks data freshness across the DAG and automatically propagates it through models materialized as views. Unlike state-aware orchestration's `build_after` config which compares against the model's last successful execution, dbt State's `lag_tolerance` compares against the freshness of the underlying data.
* **Advanced change detection** — dbt State can detect and ignore file modifications that don't change actual transformation logic, such as adding a comment or cleaning up whitespace.

If you were using state-aware orchestration prior to June 1, 2026, you can continue using it. Your dbt State trial will be extended until the billing period begins on September 1, 2026. If your trial wasn't extended, contact your account team. For details on billing after the trial ends, refer to [dbt State usage and pricing](https://docs.getdbt.com/docs/platform/billing.md#dbt-state-usage).

While dbt State is in preview, there is no required migration timeline — dbt Labs will communicate a timeline when dbt State reaches general availability.

## Migrating your configuration[​](#migrating-your-configuration "Direct link to Migrating your configuration")

Much of dbt State's configuration will feel familiar if you've used state-aware orchestration, but there is one significant difference: the `build_after` configs have moved out of the `freshness` block and into a new `state` block.

To migrate to dbt State, move your configs from `freshness.build_after` to the new `state` block. Refer to the following table for the full mapping.

| State-aware orchestration                                      | dbt State                                                                                                        | Notes                                                                                                                                                                                               |
| -------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `freshness.build_after.updates_on`                             | [`state.require_fresh_data_from`](https://docs.getdbt.com/reference/resource-configs/require-fresh-data-from.md) | Same `any` and `all` options with the same behavior:<br />- `any` (default): rebuilds when *any* direct parent has fresh data<br />- `all`: rebuilds only when *all* direct parents have fresh data |
| `freshness.build_after.count` + `freshness.build_after.period` | [`state.lag_tolerance`](https://docs.getdbt.com/reference/resource-configs/lag-tolerance.md)                     | Combined into a single field with shorthand values (for example, `1800s`, `30m`, `12h`, `1d`, `2w`) or Jinja expressions                                                                            |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Backward compatibility in Fusion

In the dbt Fusion engine, you can enable dbt State without updating your project configs first.

* If `lag_tolerance` and `require_fresh_data_from` are not set, dbt State falls back to your existing `build_after` configs until `build_after` is deprecated.
* If neither `build_after` nor the `state` configs exist, dbt State uses its [default configs](https://docs.getdbt.com/reference/resource-configs/dbt-state-configs.md): `lag_tolerance: 45m` and `require_fresh_data_from: any`.

dbt Labs will communicate a migration timeline for state-aware orchestration users when dbt State reaches general availability.

### Examples[​](#examples "Direct link to Examples")

You can set these configs at the project level in `dbt_project.yml` or at the model level in a `.yml` file.

**Project-level:**

**Before (state-aware orchestration)**

dbt\_project.yml

```yaml
models:
  +freshness:
    build_after:
      count: 1
      period: day
      updates_on: any
```

**After (dbt State)**

dbt\_project.yml

```yaml
models:
  +state:
    lag_tolerance: 1d
    require_fresh_data_from: any
```

**Model-level:**

**Before (state-aware orchestration)**

models/my\_model.yml

```yaml
models:
  - name: my_model
    config:
      freshness:
        build_after:
          count: 1
          period: day
          updates_on: any
```

**After (dbt State)**

models/my\_model.yml

```yaml
models:
  - name: my_model
    config:
      state:
        lag_tolerance: 1d
        require_fresh_data_from: any
```

## Testing the migration[​](#testing-the-migration "Direct link to Testing the migration")

You can run both configs side by side while validating. State-aware orchestration reads `freshness.build_after` and dbt State reads the `state` block, so they won't interfere with each other. Once you're confident everything looks right, remove the `build_after` configs.

dbt\_project.yml

```yaml
models:
  +freshness:
    build_after:            # state-aware orchestration (remove once migrated)
      count: 1
      period: day
      updates_on: any
  +state:                   # dbt State
    lag_tolerance: 1d
    require_fresh_data_from: any
```

## Known differences from state-aware orchestration[​](#known-differences-from-state-aware-orchestration "Direct link to Known differences from state-aware orchestration")

State-aware orchestration and dbt State differ in a few ways:

* **More models rebuilding than expected**: If you notice more rebuilds after you migrate, the most common causes are:

  * **Views with `select *`**: dbt State can't determine which columns `select *` resolves to without querying the upstream schema, so it always rebuilds these views rather than risk reusing a stale result.
  * **Non-determinism in Jinja-templated SQL**: Macros like `dbt_utils.get_relations_by_pattern` with `dbt_utils.union_relations` can return relations in a different order on each run, which produces different compiled SQL. dbt State detects a new hash and rebuilds the model. If that model has downstream dependencies, those models rebuild, too.

  Refer to [Why is my model being rebuilt instead of reused?](https://docs.getdbt.com/faqs/State/views-rebuilt.md) for details on each cause and how to diagnose them.

* **`build_after` vs `lag_tolerance`**: Both configs reduce how often a model runs when upstream data is frequently fresh, but they work differently:

  * `freshness.build_after` (for example, `{count: 4, period: hour}`) skips the model unless the configured interval has elapsed *and* upstream sources have new data since the last run. A SQL change alone does not trigger a rebuild; both conditions must be met.
  * `state.lag_tolerance` (for example, `4h`) skips the model unless upstream data is newer than the model's last run by at least the configured interval. Unlike `build_after`, a detected SQL change triggers a rebuild.

* **Efficient Testing not yet available**: State-aware orchestration offers Efficient Testing (private beta); dbt State doesn't support it yet.

## Related docs[​](#related-docs "Direct link to Related docs")

* [About dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md)
* [Set up dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-setup.md)
* [dbt State configs](https://docs.getdbt.com/reference/resource-configs/dbt-state-configs.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
