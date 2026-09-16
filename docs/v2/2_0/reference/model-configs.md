# Model configurations

## Related documentation

* [Models](../docs/build/models.md)
* [`run` command](./commands/run.md)

## Available configurations

### Model-specific configurations

Resource-specific configurations are applicable to only one dbt resource type rather than multiple resource types. You can define these settings in the project file (`dbt_project.yml`), a property file (`models/properties.yml` for models, similarly for other resources), or within the resource’s file using the `{{ config() }}` macro.

The following resource-specific configurations are only available to Models:

#### Project file

dbt\_project.yml

(Applies to dbt v1.12 and later)

```yaml
models:
  <resource-path>:
    +materialized: <materialization_name>
    +sql_header: <string>
    +on_configuration_change: apply | continue | fail # only for materialized views on supported adapters
    +unique_key: <column_name_or_expression>
    +freshness: <dict>
    +on_error: skip_children | continue
    +latest_version_pointer: <dict>
```

#### Property file

(Applies to dbt v2.0 and later)

models/properties.yml

```yaml

models:
  - name: [<model-name>]
    config:
      materialized: <materialization_name>
      sql_header: <string>
      on_configuration_change: apply | continue | fail # only for materialized views on supported adapters
      unique_key: <column_name_or_expression>
      freshness:
        warn_after: {count: 24, period: hour}
        error_after: {count: 48, period: hour}
        build_after: <dict>        # build scheduling — Enterprise only
      loaded_at_field: updated_at  # or loaded_at_query
      on_error: skip_children | continue
      latest_version_pointer: <dict>
```

#### SQL file config

models/\<model\_name>.sql

(Applies to dbt v1.12 and later)

```sql

{{ config(
    materialized="<materialization_name>",
    sql_header="<string>"
    on_configuration_change: apply | continue | fail # only for materialized views for supported adapters
    unique_key='column_name_or_expression'
    freshness=<dict>
    on_error="skip_children" | "continue"
    latest_version_pointer=<dict>
) }}
```

### General configurations

General configurations provide broader operational settings applicable across multiple resource types. Like resource-specific configurations, these can also be set in the project file, property files, or within resource-specific files.

#### Project file

dbt\_project.yml

(Applies to dbt v1.9 and later)

```yaml
models:
  <resource-path>:
    +enabled: true | false
    +tags: <string> | [<string>]
    +pre-hook: <sql-statement> | [<sql-statement>]
    +post-hook: <sql-statement> | [<sql-statement>]
    +database: <string>
    +schema: <string>
    +alias: <string>
    +persist_docs: <dict>
    +full_refresh: <boolean>
    +meta: {<dictionary>}
    +grants: {<dictionary>}
    +contract: {<dictionary>}
    +event_time: my_time_field
```

#### Property file

models/properties.yml

(Applies to dbt v1.9 and later)

```yaml

models:
  - name: [<model-name>]
    config:
      enabled: true | false
      tags: <string> | [<string>]
      pre_hook: <sql-statement> | [<sql-statement>]
      post_hook: <sql-statement> | [<sql-statement>]
      database: <string>
      schema: <string>
      alias: <string>
      persist_docs: <dict>
      full_refresh: <boolean>
      meta: {<dictionary>}
      grants: {<dictionary>}
      contract: {<dictionary>}
      event_time: my_time_field
```

#### SQL file config

models/\<model\_name>.sql

(Applies to dbt v1.9 and later)

```sql

{{ config(
    enabled=true | false,
    tags="<string>" | ["<string>"],
    pre_hook="<sql-statement>" | ["<sql-statement>"],
    post_hook="<sql-statement>" | ["<sql-statement>"],
    database="<string>",
    schema="<string>",
    alias="<string>",
    persist_docs={<dict>},
    meta={<dict>},
    grants={<dict>},
    contract={<dictionary>},
    event_time='my_time_field',

) }}
```

### Warehouse-specific configurations

* [BigQuery configurations](./resource-configs/bigquery-configs.md)
* [Redshift configurations](./resource-configs/redshift-configs.md)
* [Snowflake configurations](./resource-configs/snowflake-configs.md)
* [Databricks configurations](./resource-configs/databricks-configs.md)
* [Spark configurations](./resource-configs/spark-configs.md)

## Configuring models

Model configurations are applied hierarchically. You can configure models from within an installed package and also from within your dbt project in the following ways, listed in order of precedence:

1. Using a `config()` Jinja macro within a model.
2. Using a `config` [resource property](./model-properties.md) in a `.yml` file.
3. From the project YAML file (`dbt_project.yml`), under the `models:` key. In this case, the model that's nested the deepest will have the highest priority.

The most specific configuration always takes precedence. In the project YAML file, for example, configurations applied to a `marketing` subdirectory will take precedence over configurations applied to the entire `jaffle_shop` project. To apply a configuration to a model or directory of models, define the [resource path](./resource-configs/resource-path.md) as nested dictionary keys.

Model configurations in your root dbt project have *higher* precedence than configurations in installed packages. This enables you to override the configurations of installed packages, providing more control over your dbt runs.

## Example

### Configuring directories of models in `dbt_project.yml`

To configure models in your `dbt_project.yml` file, use the `models:` configuration option. Be sure to namespace your configurations to your project (shown below):

dbt\_project.yml

```yml


name: dbt_labs

models:
  # Be sure to namespace your model configs to your project name
  dbt_labs:

    # This configures models found in models/events/
    events:
      +enabled: true
      +materialized: view

      # This configures models found in models/events/base
      # These models will be ephemeral, as the config above is overridden
      base:
        +materialized: ephemeral

      ...
```

### Apply configurations to one model only

Some types of configurations are specific to a particular model. In these cases, placing configurations in the `dbt_project.yml` file can be unwieldy. Instead, you can specify these configurations at the top of a model `.sql` file, or in its individual YAML properties.

models/events/base/base\_events.sql

```sql
{{
  config(
    materialized = "table",
    tags = ["core", "events"]
  )
}}


select * from {{ ref('raw_events') }}
```

models/events/base/properties.yml

```yaml

models:
  - name: base_events
    description: "Standardized event data from raw sources"
    columns:
      - name: user_id
        description: "Unique identifier for a user"
        data_tests:
          - not_null
          - unique
      - name: event_type
        description: "Type of event recorded (click, purchase, etc.)"
```

(Applies to dbt v2.0 and later)

### Configuring model freshness [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

You can use the `freshness` config in the following ways:

* **Freshness SLA** (`warn_after`, `error_after`): To declare how stale this model's data is allowed to be. dbt evaluates these thresholds when you run [`dbt freshness`](./commands/freshness.md) and reports `Pass`, `Warn`, or `Error`. Support varies by materialization and measurement method. Refer to the [measurement methods table](./commands/freshness.md#what-gets-checked) for more information.

* **Build scheduling** (`build_after`): To skip rebuilding a model when upstream data hasn't changed since the last build. Available on dbt platform Enterprise tiers only.

models/my\_model.yml

```yml
models:
  - name: stg_orders
    config:
      freshness:
        # Freshness SLA — report warn/error when data is stale
        warn_after: {count: 24, period: hour}
        error_after: {count: 48, period: hour}
        # Build scheduling — Enterprise only
        build_after:
          count: 6
          period: hour
          updates_on: any  # optional
      loaded_at_field: updated_at  # or loaded_at_query
```
