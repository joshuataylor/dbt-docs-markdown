# batch\_size

Did you know\...

Available from dbt v1.9 or with the [dbt "Latest" release track](../../docs/dbt-versions/dbt-release-tracks.md).

## Definition

The `batch_size` config determines how large batches are when running a [microbatch incremental model](../../docs/build/incremental-microbatch.md). Accepted values are `hour`, `day`, `month`, or `year`. You can configure `batch_size` for a [model](../../docs/build/models.md) in your project YAML file (`dbt_project.yml`), properties YAML file, or config block.

## Examples

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
