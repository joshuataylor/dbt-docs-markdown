# concurrent\_batches

💡Did you know\...

Available from dbt v

<!-- -->

1.9

<!-- -->

or with the

<!-- -->

[dbt "Latest" release track](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md).

* Project YAML file
* SQL file config

dbt\_project.yml

```yaml
models:
  +concurrent_batches: true
```

models/my\_model.sql

```sql
{{
  config(
    materialized='incremental',
    concurrent_batches=true,
    incremental_strategy='microbatch'
        ...
  )
}}
select ...
```

## Definition[​](#definition "Direct link to Definition")

`concurrent_batches` is an override which allows you to decide whether or not you want to run batches in parallel or sequentially (one at a time).

For more information, refer to [how batch execution works](https://docs.getdbt.com/docs/build/parallel-batch-execution.md#how-parallel-batch-execution-works).

## Example[​](#example "Direct link to Example")

By default, dbt auto-detects whether batches can run in parallel for microbatch models. However, you can override dbt's detection by setting the `concurrent_batches` config to `false` in your `dbt_project.yml` or model `.sql` file to specify parallel or sequential execution, given you meet these conditions:

* You've configured a [microbatch incremental strategy](https://docs.getdbt.com/docs/build/incremental-microbatch.md).
* You're working with cumulative metrics or any logic that depends on batch order.

Set `concurrent_batches` config to `false` to ensure batches are processed sequentially. For example:

dbt\_project.yml

```yaml
models:
  my_project:
    cumulative_metrics_model:
      +concurrent_batches: false
```

models/my\_model.sql

```sql
{{
  config(
    materialized='incremental',
    incremental_strategy='microbatch'
    concurrent_batches=false
  )
}}
select ...
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
