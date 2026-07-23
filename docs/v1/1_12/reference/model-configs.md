# Model configurations

## Related documentation[​](#related-documentation "Direct link to Related documentation")

* [Models](https://docs.getdbt.com/docs/build/models.md)
* [`run` command](https://docs.getdbt.com/reference/commands/run.md)

## Available configurations[​](#available-configurations "Direct link to Available configurations")

### Model-specific configurations[​](#model-specific-configurations "Direct link to Model-specific configurations")

Resource-specific configurations are applicable to only one dbt resource type rather than multiple resource types. You can define these settings in the project file (`dbt_project.yml`), a property file (`models/properties.yml` for models, similarly for other resources), or within the resource’s file using the `{{ config() }}` macro.<br />

The following resource-specific configurations are only available to <!-- -->Models:

* Project file
* Property file
* SQL file config

dbt\_project.yml

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

Note, most model configurations are defined under `config`, while `build_after` is set under `freshness`.

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
        # build_after is nested under freshness. Available on dbt platform Enterprise tiers only.
        build_after: <dict>
      on_error: skip_children | continue
      latest_version_pointer: <dict>
```

models/\<model\_name>.sql

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

### General configurations[​](#general-configurations "Direct link to General configurations")

General configurations provide broader operational settings applicable across multiple resource types. Like resource-specific configurations, these can also be set in the project file, property files, or within resource-specific files.

* Project file
* Property file
* SQL file config

dbt\_project.yml

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

models/properties.yml

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

models/\<model\_name>.sql

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

### Warehouse-specific configurations[​](#warehouse-specific-configurations "Direct link to Warehouse-specific configurations")

* [BigQuery configurations](https://docs.getdbt.com/reference/resource-configs/bigquery-configs.md)
* [Redshift configurations](https://docs.getdbt.com/reference/resource-configs/redshift-configs.md)
* [Snowflake configurations](https://docs.getdbt.com/reference/resource-configs/snowflake-configs.md)
* [Databricks configurations](https://docs.getdbt.com/reference/resource-configs/databricks-configs.md)
* [Spark configurations](https://docs.getdbt.com/reference/resource-configs/spark-configs.md)

## Configuring models[​](#configuring-models "Direct link to Configuring models")

Model configurations are applied hierarchically. You can configure models from within an installed package and also from within your dbt project in the following ways, listed in order of precedence:

1. Using a `config()` Jinja macro within a model.
2. Using a `config` [resource property](https://docs.getdbt.com/reference/model-properties.md) in a `.yml` file.
3. From the project YAML file (`dbt_project.yml`), under the `models:` key. In this case, the model that's nested the deepest will have the highest priority.

The most specific configuration always takes precedence. In the project YAML file, for example, configurations applied to a `marketing` subdirectory will take precedence over configurations applied to the entire `jaffle_shop` project. To apply a configuration to a model or directory of models, define the [resource path](https://docs.getdbt.com/reference/resource-configs/resource-path.md) as nested dictionary keys.

Model configurations in your root dbt project have *higher* precedence than configurations in installed packages. This enables you to override the configurations of installed packages, providing more control over your dbt runs.

## Example[​](#example "Direct link to Example")

### Configuring directories of models in `dbt_project.yml`[​](#configuring-directories-of-models-in-dbt_projectyml "Direct link to configuring-directories-of-models-in-dbt_projectyml")

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

### Apply configurations to one model only[​](#apply-configurations-to-one-model-only "Direct link to Apply configurations to one model only")

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

### Configuring source freshness[​](#configuring-source-freshness "Direct link to Configuring source freshness")

The model `freshness` config rebuilds models only when new source or upstream data is available. This is useful for models that depend on other models but only need to be updated periodically. For more information, see [freshness](https://docs.getdbt.com/reference/resource-configs/freshness.md).

Note that for every `freshness` config, you're required to either set values for both `count` and `period`, or set `freshness: null`. This requirement applies to all `freshness` types: `freshness.warn_after`, `freshness.error_after`, and `freshness.build_after`.

See the following example of a `my_model.yml` file using the `freshness` config:

models/my\_model.yml

```yml
models:
  - name: stg_orders
    config:
      freshness:
        build_after:  # build this model no more often than every X amount of time, as long as it has new data. Available only on dbt platform Enterprise tiers. 
          count: <positive_integer>
          period: minute | hour | day
          updates_on: any | all # optional config
```
