# materialized

* Project YAML file
* Properties YAML file
* SQL file config

dbt\_project.yml

```yaml
config-version: 2

models:
  <resource-path>:
    +materialized: <materialization_name>
```

models/properties.yml

```yaml

models:
  - name: <model_name>
    config:
      materialized: <materialization_name>
```

models/\<model\_name>.sql

```jinja
{{ config(
  materialized="<materialization_name>"
) }}

select ...
```

## Definition[​](#definition "Direct link to Definition")

[Materializations](https://docs.getdbt.com/docs/build/materializations.md#materializations) are strategies for persisting dbt models in a warehouse. These are the materialization types built into dbt:

* `ephemeral` — [ephemeral](https://docs.getdbt.com/docs/build/materializations.md#ephemeral) models are not directly built into the database
* `table` — a model is rebuilt as a [table](https://docs.getdbt.com/docs/build/materializations.md#table) on each run
* `view` — a model is rebuilt as a [view](https://docs.getdbt.com/docs/build/materializations.md#view) on each run
* `materialized_view` — allows the creation and maintenance of [materialized views](https://docs.getdbt.com/docs/build/materializations.md#materialized-view) in the target database
* `incremental` — [incremental](https://docs.getdbt.com/docs/build/materializations.md#incremental) models allow dbt to insert or update records into a table since the last time that model was run

You can also configure [custom materializations](https://docs.getdbt.com/guides/create-new-materializations.md?step=1) in dbt. Custom materializations are a powerful way to extend dbt's functionality to meet your specific needs.

## Creation Precedence[​](#creation-precedence "Direct link to Creation Precedence")

Materializations are implemented following this "drop through" life cycle:

1. If a model does not exist with the provided path, create the new model.
2. If a model exists, but has a different type, drop the existing model and create the new model.
3. If [`--full-refresh`](https://docs.getdbt.com/reference/resource-configs/full_refresh.md) is supplied, replace the existing model regardless of configuration changes and the [`on_configuration_change`](https://docs.getdbt.com/reference/resource-configs/on_configuration_change.md) setting.
4. If there are no configuration changes, perform the default action for that type (e.g. apply refresh for a materialized view).
5. Determine whether to apply the configuration changes according to the `on_configuration_change` setting.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
