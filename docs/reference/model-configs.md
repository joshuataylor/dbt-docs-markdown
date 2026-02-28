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

models/\<model\_name>.sql

### General configurations[​](#general-configurations "Direct link to General configurations")

General configurations provide broader operational settings applicable across multiple resource types. Like resource-specific configurations, these can also be set in the project file, property files, or within resource-specific files.

* Project file
* Property file
* SQL file config

dbt\_project.yml

models/properties.yml

models/\<model\_name>.sql

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

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
