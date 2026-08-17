# begin

💡Did you know\...

Available from dbt v1.9 or with the [dbt "Latest" release track](../../docs/dbt-versions/dbt-release-tracks.md).

## Definition

Set the `begin` config to the timestamp value at which your [microbatch incremental model](../../docs/build/incremental-microbatch.md) data should begin — at the point the data becomes relevant for the microbatch model.

You can configure `begin` for a [model](../../docs/build/models.md) in your project YAML file (`dbt_project.yml`), properties YAML file, or SQL file config. The value for `begin` must be a string representing an ISO-formatted date, *or* date and time, *or* [relative dates](#set-begin-to-use-relative-dates). Check out the [examples](#examples) in the next section for more details.

## Examples

The following examples set `2024-01-01 00:00:00` as the `begin` config for the `user_sessions` model.

#### Example in the `dbt_project.yml` file

dbt\_project.yml

```yml
models:
  my_project:
    user_sessions:
      +begin: "2024-01-01 00:00:00"
```

#### Example in a property YAML file

models/properties.yml

```yml
models:
  - name: user_sessions
    config:
      begin: "2024-01-01 00:00:00"
```

#### Example in a SQL config block for a model

models/user\_sessions.sql

```sql
{{ config(
    begin='2024-01-01 00:00:00'
) }}
```

#### Set `begin` to use relative dates

To configure `begin` to use relative dates, you can use modules variables [`modules.datetime`](../dbt-jinja-functions/modules.md#datetime) and [`modules.pytz`](../dbt-jinja-functions/modules.md#pytz) to dynamically specify relative timestamps, such as yesterday's date or the start of the current week.

For example, to set `begin` to yesterday's date:

```sql
{{
    config(
        materialized = 'incremental',
        incremental_strategy='microbatch',
        unique_key = 'run_id',
        begin=(modules.datetime.datetime.now() - modules.datetime.timedelta(1)).isoformat(),
        event_time='created_at',
        batch_size='day',
    )
}}
```
