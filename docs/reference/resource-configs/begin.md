# begin

💡Did you know\...

Available from dbt v

<!-- -->

1.9

<!-- -->

or with the

<!-- -->

[dbt "Latest" release track](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md).

## Definition[​](#definition "Direct link to Definition")

Set the `begin` config to the timestamp value at which your [microbatch incremental model](https://docs.getdbt.com/docs/build/incremental-microbatch.md) data should begin — at the point the data becomes relevant for the microbatch model.

You can configure `begin` for a [model](https://docs.getdbt.com/docs/build/models.md) in your project YAML file (`dbt_project.yml`), properties YAML file, or SQL file config. The value for `begin` must be a string representing an ISO-formatted date, *or* date and time, *or* [relative dates](#set-begin-to-use-relative-dates). Check out the [examples](#examples) in the next section for more details.

## Examples[​](#examples "Direct link to Examples")

The following examples set `2024-01-01 00:00:00` as the `begin` config for the `user_sessions` model.

#### Example in the `dbt_project.yml` file[​](#example-in-the-dbt_projectyml-file "Direct link to example-in-the-dbt_projectyml-file")

dbt\_project.yml

```yml
models:
  my_project:
    user_sessions:
      +begin: "2024-01-01 00:00:00"
```

#### Example in a property YAML file[​](#example-in-a-property-yaml-file "Direct link to Example in a property YAML file")

models/properties.yml

```yml
models:
  - name: user_sessions
    config:
      begin: "2024-01-01 00:00:00"
```

#### Example in a SQL config block for a model[​](#example-in-a-sql-config-block-for-a-model "Direct link to Example in a SQL config block for a model")

models/user\_sessions.sql

```sql
{{ config(
    begin='2024-01-01 00:00:00'
) }}
```

#### Set `begin` to use relative dates[​](#set-begin-to-use-relative-dates "Direct link to set-begin-to-use-relative-dates")

To configure `begin` to use relative dates, you can use modules variables [`modules.datetime`](https://docs.getdbt.com/reference/dbt-jinja-functions/modules.md#datetime) and [`modules.pytz`](https://docs.getdbt.com/reference/dbt-jinja-functions/modules.md#pytz) to dynamically specify relative timestamps, such as yesterday's date or the start of the current week.

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

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
