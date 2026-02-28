# on\_configuration\_change

info

This functionality is currently only supported for [materialized views](https://docs.getdbt.com/docs/build/materializations.md#materialized-view) on a subset of adapters.

The `on_configuration_change` config has three settings:

* `apply` (default) — Attempt to update the existing database object if possible, avoiding a complete rebuild.
  <!-- -->
  * *Note:* If any individual configuration change requires a full refresh, a full refresh is performed in lieu of individual alter statements.
* `continue` — Allow runs to continue while also providing a warning that the object was left untouched.
  <!-- -->
  * *Note:* This could result in downstream failures as those models may depend on these unimplemented changes.
* `fail` — Force the entire run to fail if a change is detected.

- Project YAML file
- Properties YAML file
- SQL file config

dbt\_project.yml

```yaml
models:
  <resource-path>:
    +materialized: <materialization_name>
    +on_configuration_change: apply | continue | fail
```

models/properties.yml

```yaml

models:
  - name: [<model-name>]
    config:
      materialized: <materialization_name>
      on_configuration_change: apply | continue | fail
```

models/\<model\_name>.sql

```jinja
{{ config(
    materialized="<materialization_name>",
    on_configuration_change="apply" | "continue" | "fail"
) }}
```

Materializations are implemented following this "drop through" life cycle:

1. If a model does not exist with the provided path, create the new model.
2. If a model exists, but has a different type, drop the existing model and create the new model.
3. If [`--full-refresh`](https://docs.getdbt.com/reference/resource-configs/full_refresh.md) is supplied, replace the existing model regardless of configuration changes and the `on_configuration_change` setting.
4. If there are no configuration changes, perform the default action for that type (e.g. apply refresh for a materialized view).
5. Determine whether to apply the configuration changes according to the `on_configuration_change` setting.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
