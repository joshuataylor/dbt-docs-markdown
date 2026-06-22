# Redshift configurations

## Incremental materialization strategies[​](#incremental-materialization-strategies "Direct link to Incremental materialization strategies")

In dbt-redshift, the following incremental materialization strategies are supported:

* `append` (default when `unique_key` is not defined)
* `merge`
* `delete+insert` (default when `unique_key` is defined)
* [`microbatch`](https://docs.getdbt.com/docs/build/incremental-microbatch.md)

All of these strategies are inherited from dbt-postgres.

## Performance optimizations[​](#performance-optimizations "Direct link to Performance optimizations")

### Using sortkey and distkey[​](#using-sortkey-and-distkey "Direct link to Using sortkey and distkey")

Tables in Amazon Redshift have two powerful optimizations to improve query performance: distkeys and sortkeys. Supplying these values as model-level configurations apply the corresponding settings in the generated `CREATE TABLE` DDL. Note that these settings will have no effect on models set to `view` or `ephemeral` models.

* `dist` can have a setting of `all`, `even`, `auto`, or the name of a key.
* `sort` accepts a list of sort keys, for example: `['reporting_day', 'category']`. dbt will build the sort key in the same order the fields are supplied.
* `sort_type` can have a setting of `interleaved` or `compound`. if no setting is specified, sort\_type defaults to `compound`.

When working with sort keys, it's highly recommended you follow [Redshift's best practices](https://docs.aws.amazon.com/prescriptive-guidance/latest/query-best-practices-redshift/best-practices-tables.html#sort-keys) on sort key effectiveness and cardinality.

Sort and dist keys should be added to the `{{ config(...) }}` block in model `.sql` files, eg:

my\_model.sql

```sql
-- Example with one sort key
{{ config(materialized='table', sort='reporting_day', dist='unique_id') }}

select ...


-- Example with multiple sort keys
{{ config(materialized='table', sort=['category', 'region', 'reporting_day'], dist='received_at') }}

select ...


-- Example with interleaved sort keys
{{ config(materialized='table',
          sort_type='interleaved'
          sort=['category', 'region', 'reporting_day'],
          dist='unique_id')
}}

select ...
```

For more information on distkeys and sortkeys, view Amazon's docs:

* [AWS Documentation » Amazon Redshift » Database Developer Guide » Designing Tables » Choosing a Data Distribution Style](https://docs.aws.amazon.com/redshift/latest/dg/t_Distributing_data.html)
* [AWS Documentation » Amazon Redshift » Database Developer Guide » Designing Tables » Choosing Sort Keys](https://docs.aws.amazon.com/redshift/latest/dg/t_Sorting_data.html)

### Session configuration[​](#session-configuration "Direct link to Session configuration")

The Redshift adapter supports the `query_group` session parameter, enabling dbt runs to tag queries for Redshift Workload Manager (WLM) and query logging (for example, `STL_QUERY` and `SVL_QLOG`). You can set `query_group` at the profile level (default for the connection) and override it at the model level.

* **Profile-level configuration**

  Configure `query_group` in your `profiles.yml` to apply a default value to all queries executed using that profile. dbt sets the `query_group` when opening a connection.

  profiles.yml

  ```yml
  outputs:
    dev:
      type: redshift
      host: CLUSTER_ENDPOINT
      user: REDSHIFT_USER
      password: REDSHIFT_PASSWORD
      dbname: REDSHIFT_DBNAME
      port: 5439
      schema: analytics
      threads: 4
      query_group: QUERY_GROUP_NAME
  ```

  ```sql
  -- models/a_default_group.sql
  -- Runs under query_group = 'QUERY_GROUP_NAME' (from the profile)
  select 1 as id
  ```

* **Model-level configuration**

  Set `query_group` in a model's `config()` block to temporarily override the default value for that model’s execution. dbt applies the model-level value while the model runs and then restores the default value after model materialization.

  ```sql
  -- models/b_override_group.sql
  -- dbt temporarily sets query_group = 'dbt_finance' for this model, then restores the default value
  {{ config(query_group='dbt_finance') }}

  select 1 as id
  ```

## Datasharing [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[​](#datasharing- "Direct link to datasharing-")

Previously, the Redshift adapter used PostgreSQL-compatible catalog tables (for example, `pg_*`, `information_schema`) for metadata operations such as listing relations, schemas, and columns. These tables only surface objects within the currently connected database, which prevents cross-database operations needed for [Redshift Datasharing](https://docs.aws.amazon.com/redshift/latest/dg/datashare-overview.html).

Starting `dbt-redshift` v1.11.0rc1, you can set `datasharing: true` in your `profiles.yml` to enable cross-database and cross-cluster access. When enabled, `dbt-redshift` switches metadata queries to Redshift's native `SHOW` system commands. You can then materialize models into a database or cluster other than the one specified in your profile using `{{ config(database='other_db') }}`.

Example configuration:

profiles.yml

```yml
company-name:
  target: dev
  outputs:
    dev:
      type: redshift
      host: hostname.region.redshift.amazonaws.com
      ...
      datasharing: true  # default: false
```

Once enabled, you can materialize a model into a different database by setting `database` in the model config. For example:

```sql
{{ config(database='other_db') }}

select * from {{ ref('my_model') }}
```

The following macros switch to `SHOW` commands when `datasharing: true`:

| Macro                                 | Without `datasharing`               | With `datasharing`                                 |
| ------------------------------------- | ----------------------------------- | -------------------------------------------------- |
| `list_relations_without_caching`      | `information_schema.tables`         | `SHOW TABLES FROM SCHEMA`                          |
| `list_schemas`, `check_schema_exists` | `pg_namespace`                      | `SHOW SCHEMAS FROM DATABASE`                       |
| `get_columns_in_relation`             | `information_schema.columns`        | `SHOW COLUMNS FROM TABLE`                          |
| Catalog queries                       | `pg_class`, `pg_tables`, `pg_views` | `SHOW TABLES FROM SCHEMA` + `SVV_REDSHIFT_COLUMNS` |
| `get_relation_last_modified`          | `information_schema.tables`         | `SHOW TABLES FROM SCHEMA`                          |
| Grants                                | `pg_user`, `has_table_privilege()`  | `SHOW GRANTS ON TABLE`                             |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

`ra3_node: true` also enables this behavior and is supported for backward compatibility. For new projects, use `datasharing: true` instead.

note

If you see `pg_*` queries running with `datasharing` enabled, this is not necessarily a bug. Only the macros listed above are migrated — some macros remain on `pg_*` by design (for example, `get_relations` for dependency tracking and `list_function_relations_without_caching` for function discovery). Custom macro overrides in your project are not affected. To confirm whether a `pg_*` query is expected, check which macro is being overridden and which metadata operation is running.

The following limitations apply when using `datasharing`:

* Creating views (including materialized views) in another database is not supported.
* Cross-database grants on objects are not supported.
* Source freshness checks can have a lag of up to 5 minutes.
* Metadata queries are limited to 10,000 rows. If a database has more than 10,000 schemas, or a schema has more than 10,000 tables, dbt runs can result in unexpected scenarios.
* Cross-database writes require the `SNAPSHOT` transaction isolation level.
* For views that reference tables in another database, define them as [late-binding views](https://docs.getdbt.com/reference/resource-configs/redshift-configs.md#late-binding-views).

For setup instructions, see [Redshift setup](https://docs.getdbt.com/docs/local/connect-data-platform/redshift-setup.md).

## Late binding views[​](#late-binding-views "Direct link to Late binding views")

Redshift supports views unbound from their dependencies, or [late binding views](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_VIEW.html#late-binding-views). This DDL option "unbinds" a view from the data it selects from. In practice, this means that if upstream views or tables are dropped with a cascade qualifier, the late-binding view does not get dropped as well.

Using late-binding views in a production deployment of dbt can vastly improve the availability of data in the warehouse, especially for models that are materialized as late-binding views and are queried by end-users, since they won’t be dropped when upstream models are updated. Additionally, late binding views can be used with [external tables](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_EXTERNAL_TABLE.html) via Redshift Spectrum.

To materialize a dbt model as a late binding view, use the `bind: false` configuration option:

my\_view\.sql

```sql
{{ config(materialized='view', bind=False) }}

select *
from source.data
```

To make all views late-binding, configure your `dbt_project.yml` file like this:

dbt\_project.yml

```yaml
models:
  +bind: false # Materialize all views as late-binding
  project_name:
    ....
```

## Materialized views[​](#materialized-views "Direct link to Materialized views")

The Redshift adapter supports [materialized views](https://docs.aws.amazon.com/redshift/latest/dg/materialized-view-overview.html) with the following configuration parameters:

| Parameter                                                                                                  | Type         | Required | Default                                       | Change Monitoring Support |
| ---------------------------------------------------------------------------------------------------------- | ------------ | -------- | --------------------------------------------- | ------------------------- |
| [`on_configuration_change`](https://docs.getdbt.com/reference/resource-configs/on_configuration_change.md) | `<string>`   | no       | `apply`                                       | n/a                       |
| [`dist`](#using-sortkey-and-distkey)                                                                       | `<string>`   | no       | `even`                                        | drop/create               |
| [`sort`](#using-sortkey-and-distkey)                                                                       | `[<string>]` | no       | `none`                                        | drop/create               |
| [`sort_type`](#using-sortkey-and-distkey)                                                                  | `<string>`   | no       | `auto` if no `sort`<br />`compound` if `sort` | drop/create               |
| [`auto_refresh`](#auto-refresh)                                                                            | `<boolean>`  | no       | `false`                                       | alter                     |
| [`backup`](#backup)                                                                                        | `<string>`   | no       | `true`                                        | n/a                       |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

* Project YAML file
* Properties YAML file
* SQL file config

dbt\_project.yml

```yaml
models:
  <resource-path>:
    +materialized: materialized_view
    +on_configuration_change: apply | continue | fail
    +dist: all | auto | even | <field-name>
    +sort: <field-name> | [<field-name>]
    +sort_type: auto | compound | interleaved
    +auto_refresh: true | false
    +backup: true | false
```

models/properties.yml

```yaml

models:
  - name: [<model-name>]
    config:
      materialized: materialized_view
      on_configuration_change: apply | continue | fail
      dist: all | auto | even | <field-name>
      sort: <field-name> | [<field-name>]
      sort_type: auto | compound | interleaved
      auto_refresh: true | false
      backup: true | false
```

models/\<model\_name>.sql

```jinja
{{ config(
    materialized="materialized_view",
    on_configuration_change="apply" | "continue" | "fail",
    dist="all" | "auto" | "even" | "<field-name>",
    sort=["<field-name>"],
    sort_type="auto" | "compound" | "interleaved",
    auto_refresh=true | false,
    backup=true | false,
) }}
```

Many of these parameters correspond to their table counterparts and have been linked above. The parameters unique to materialized views are the [auto-refresh](#auto-refresh) and [backup](#backup) functionality, which are covered below.

Learn more about these parameters in Redshift's [docs](https://docs.aws.amazon.com/redshift/latest/dg/materialized-view-create-sql-command.html).

#### Auto-refresh[​](#auto-refresh "Direct link to Auto-refresh")

| Parameter      | Type        | Required | Default | Change Monitoring Support |
| -------------- | ----------- | -------- | ------- | ------------------------- |
| `auto_refresh` | `<boolean>` | no       | `false` | alter                     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Redshift supports [automatic refresh](https://docs.aws.amazon.com/redshift/latest/dg/materialized-view-refresh.html#materialized-view-auto-refresh) configuration for materialized views. By default, a materialized view does not automatically refresh. dbt monitors this parameter for changes and applies them using an `ALTER` statement.

Learn more information about the [parameters](https://docs.aws.amazon.com/redshift/latest/dg/materialized-view-create-sql-command.html#mv_CREATE_MATERIALIZED_VIEW-parameters) in the Redshift docs.

#### Backup[​](#backup "Direct link to Backup")

| Parameter | Type        | Required | Default | Change Monitoring Support |
| --------- | ----------- | -------- | ------- | ------------------------- |
| `backup`  | `<boolean>` | no       | `true`  | n/a                       |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Redshift supports [backup](https://docs.aws.amazon.com/redshift/latest/mgmt/working-with-snapshots.html) configuration of clusters at the object level. This parameter identifies if the materialized view should be backed up as part of the cluster snapshot. By default, a materialized view will be backed up during a cluster snapshot. dbt cannot monitor this parameter as it is not queryable within Redshift. If the value changes, the materialized view will need to go through a `--full-refresh` to set it.

Learn more about these parameters in Redshift's [docs](https://docs.aws.amazon.com/redshift/latest/dg/materialized-view-create-sql-command.html#mv_CREATE_MATERIALIZED_VIEW-parameters).

### Limitations[​](#limitations "Direct link to Limitations")

As with most data platforms, there are limitations associated with materialized views. Some worth noting include:

* Materialized views cannot reference views, temporary tables, user-defined functions, or late-binding tables.
* Auto-refresh cannot be used if the materialized view references mutable functions, external schemas, or another materialized view.

Find more information about materialized view limitations in Redshift's [docs](https://docs.aws.amazon.com/redshift/latest/dg/materialized-view-create-sql-command.html#mv_CREATE_MATERIALIZED_VIEW-limitations).

## Unit test limitations[​](#unit-test-limitations "Direct link to Unit test limitations")

* Redshift doesn't support [unit tests](https://docs.getdbt.com/docs/build/unit-tests.md) when the SQL in the common table expression (CTE) contains functions such as `LISTAGG`, `MEDIAN`, `PERCENTILE_CONT`, and so on. These functions must be executed against a user-created table. dbt combines given rows to be part of the CTE, which Redshift does not support.

  In order to support this pattern in the future, dbt would need to "materialize" the input fixtures as tables, rather than interpolating them as CTEs. If you are interested in this functionality, we'd encourage you to participate in this issue in GitHub: [dbt-labs/dbt Core#8499](https://github.com/dbt-labs/dbt-core/issues/8499)

* Redshift doesn't support unit tests that rely on sources in a database that differs from the models. See this issue in GitHub for more detail: <https://github.com/dbt-labs/dbt-redshift/issues/995>

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
