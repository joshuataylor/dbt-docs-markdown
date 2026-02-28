# lookback

💡Did you know\...

Available from dbt v

<!-- -->

1.9

<!-- -->

or with the

<!-- -->

[dbt "Latest" release track](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md).

## Definition[​](#definition "Direct link to Definition")

Configure a `lookback` window to reprocess additional batches during [microbatch incremental model](https://docs.getdbt.com/docs/build/incremental-microbatch.md) runs. It processes X batches up to the latest bookmark (the last successfully processed data point) to capture late-arriving records.

Set the `lookback` to an integer greater than or equal to zero. The default value is `1`. You can configure `lookback` for a [microbatch incremental model](https://docs.getdbt.com/docs/build/incremental-microbatch.md) in your project YAML file (`dbt_project.yml`), properties YAML file (`models/properties.yml`), or SQL file config.

## Examples[​](#examples "Direct link to Examples")

The following examples set `2` as the `lookback` config for the `user_sessions` model.

Example in the `dbt_project.yml` file:

dbt\_project.yml

```yml
models:
  my_project:
    user_sessions:
      +lookback: 2
```

Example in a property file:

models/properties.yml

```yml
models:
  - name: user_sessions
    config:
      lookback: 2
```

Example in SQL config block:

models/user\_sessions.sql

```sql
{{ config(
    lookback=2
) }}
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
