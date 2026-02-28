# batch\_size

💡Did you know\...

Available from dbt v

<!-- -->

1.9

<!-- -->

or with the

<!-- -->

[dbt "Latest" release track](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md).

## Definition[​](#definition "Direct link to Definition")

The `batch_size` config determines how large batches are when running a [microbatch incremental model](https://docs.getdbt.com/docs/build/incremental-microbatch.md). Accepted values are `hour`, `day`, `month`, or `year`. You can configure `batch_size` for a [model](https://docs.getdbt.com/docs/build/models.md) in your project YAML file (`dbt_project.yml`), properties YAML file, or config block.

## Examples[​](#examples "Direct link to Examples")

The following examples set `day` as the `batch_size` for the `user_sessions` model.

Example of the `batch_size` config in the `dbt_project.yml` file:

dbt\_project.yml

```yml
models:
  my_project:
    user_sessions:
      +batch_size: day
```

Example in a property file:

models/properties.yml

```yml
models:
  - name: user_sessions
    config:
      batch_size: day
```

Example in a config block for a model:

models/user\_sessions.sql

```sql
{{ config(
    materialized='incremental',
    batch_size='day'
) }}
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
