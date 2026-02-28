# target\_database

note

Starting in dbt Core v1.9+, this functionality is no longer utilized. Use the [database](https://docs.getdbt.com/reference/resource-configs/database.md) config as an alternative to define a custom database while still respecting the `generate_database_name` macro.

Try it now in the [dbt **Latest** release track](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md).

dbt\_project.yml

```yml
snapshots:
  <resource-path>:
    +target_database: string
```

snapshots/\<filename>.sql

```jinja2
{{ config(
  target_database="string"
) }}
```

## Description[​](#description "Direct link to Description")

The database that dbt should build a [snapshot](https://docs.getdbt.com/docs/build/snapshots.md) table into.

Notes:

* The specified database must already exist
* On **BigQuery**, this is analogous to a `project`.
* On **Redshift**, cross-database queries are not possible. If you use this parameter, you will receive the following error. As such, **do not use** this parameter on Redshift:

```text
Encountered an error:
Runtime Error
  Cross-db references not allowed in redshift (raw vs analytics)
```

## Default[​](#default "Direct link to Default")

By default, dbt will use the [target](https://docs.getdbt.com/reference/dbt-jinja-functions/target.md) database associated with your profile/connection.

## Examples[​](#examples "Direct link to Examples")

### Build all snapshots in a database named `snapshots`[​](#build-all-snapshots-in-a-database-named-snapshots "Direct link to build-all-snapshots-in-a-database-named-snapshots")

dbt\_project.yml

```yml
snapshots:
  +target_database: snapshots
```

### Use a target-aware database[​](#use-a-target-aware-database "Direct link to Use a target-aware database")

Use the [`{{ target }}` variable](https://docs.getdbt.com/reference/dbt-jinja-functions/target.md) to change which database a snapshot table is built in.

Note: consider whether this use-case is right for you, as downstream `refs` will select from the `dev` version of a snapshot, which can make it hard to validate models that depend on snapshots.

dbt\_project.yml

```yml
snapshots:
  +target_database: "{% if target.name == 'dev' %}dev{% else %}{{ target.database }}{% endif %}"
```

### Use the same database-naming behavior as models[​](#use-the-same-database-naming-behavior-as-models "Direct link to Use the same database-naming behavior as models")

Leverage the [`generate_database_name` macro](https://docs.getdbt.com/docs/build/custom-databases.md) to build snapshots in databases that follow the same naming behavior as your models.

Notes:

* This macro is not available when configuring from the `dbt_project.yml` file, so it must be configured in a snapshot SQL file config.
* Consider whether this use-case is right for you, as downstream `refs` will select from the `dev` version of a snapshot, which can make it hard to validate models that depend on snapshots.

snapshots/orders\_snaphot.sql

```sql
{{
    config(
      target_database=generate_database_name('snapshots')
    )
}}
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
