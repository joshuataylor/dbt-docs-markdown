# Putting it together

```bash
dbt run --select "my_package.*+"      # select all models in my_package and their children
dbt run --select "+some_model+"       # select some_model and all parents and children

dbt run --select "tag:nightly+"      # select "nightly" models and all children
dbt run --select "+tag:nightly+"      # select "nightly" models and all parents and children

dbt run --select "@source:snowplow"   # build all models that select from snowplow sources, plus their parents

dbt test --select "config.incremental_strategy:insert_overwrite,test_name:unique"   # execute all `unique` tests that select from models using the `insert_overwrite` incremental strategy
```

This can get complex! Let's say I want a nightly run of models that build off snowplow data and feed exports, while *excluding* the biggest incremental models (and one other model, to boot).

```bash
dbt run --select "@source:snowplow,tag:nightly models/export" --exclude "package:snowplow,config.materialized:incremental export_performance_timing"
```

This command selects all models that:

* Select from snowplow sources, plus their parents, *and* are tagged "nightly"
* Are defined in the `export` model subfolder

Except for models that are:

* Defined in the snowplow package and materialized incrementally
* Named `export_performance_timing`

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
