# meta

* Models
* Sources
* Seeds
* Snapshots
* Tests
* Unit tests
* Analyses
* Macros
* Exposures
* Semantic models
* Metrics
* Saved queries

dbt\_project.yml

```yml
models:
  <resource-path>:
    +meta: {<dictionary>}
```

models/schema.yml

```yml

models:
  - name: model_name
    config:
      meta: {<dictionary>}

    columns:
      - name: column_name
        config:
          meta: {<dictionary>} # changed to config in v1.10 and backported to 1.9
```

The `meta` config can be defined:

* Under the `models` config in the project file (shown in previous 'models/schema.yml' example)
* Under the `models` config in the project file (`dbt_project.yml`)
* in a `config()` Jinja macro within a model's SQL file

See [configs and properties](https://docs.getdbt.com/reference/configs-and-properties.md) for details.

dbt\_project.yml

```yml
sources:
  <resource-path>:
    +meta: {<dictionary>}
```

models/schema.yml

```yml

sources:
  - name: model_name
    config:
      meta: {<dictionary>}

    tables:
      - name: table_name
        config:
          meta: {<dictionary>}

        columns:
          - name: column_name
            config:
              meta: {<dictionary>} # changed to config in v1.10 and backported to 1.9
```

dbt\_project.yml

```yml
seeds:
  <resource-path>:
    +meta: {<dictionary>}
```

seeds/schema.yml

```yml

seeds:
  - name: seed_name
    config:
      meta: {<dictionary>}

    columns:
      - name: column_name
        config:
          meta: {<dictionary>} # changed to config in v1.10 and backported to 1.9
```

The `meta` config can be defined:

* Under the `seeds` config in the property file (shown in in previous 'seeds/schema.yml' example)
* Under the `seeds` config in the project file (`dbt_project.yml`). See [configs and properties](https://docs.getdbt.com/reference/configs-and-properties.md) for details.

dbt\_project.yml

```yml
snapshots:
  <resource-path>:
    +meta: {<dictionary>}
```

snapshots/schema.yml

```yml

snapshots:
  - name: snapshot_name
    config:
      meta: {<dictionary>}

    columns:
      - name: column_name
        config:
          meta: {<dictionary>} # changed to config in v1.10 and backported to 1.9
```

The `meta` config can be defined:

* under the `snapshots` config in the properties file (shown in previous `snapshots/schema.yml` example)
* under the `snapshots` config in the project file (`dbt_project.yml`)
* in a `config()` Jinja macro within a snapshot's SQL block

See [configs and properties](https://docs.getdbt.com/reference/configs-and-properties.md) for details.

Use the `meta` field to add metadata to [generic](https://docs.getdbt.com/docs/build/data-tests.md#generic-data-tests) or [singular tests](https://docs.getdbt.com/docs/build/data-tests.md#singular-data-tests). `meta` accepts key-value pairs, is compiled into `manifest.json`, and appears in auto-generated documentation.

**Generic data tests**

Add `meta` under the `config` block in your `properties.yml` file:

models/properties.yml

```yaml
models:
  - name: my_model
    columns:
      - name: my_column
        data_tests:
          - unique:
              config:
                meta:
                  owner: "docs team"
```

Or set defaults in `dbt_project.yml`:

dbt\_project.yml

```yaml
data_tests:
  my_project:
    +meta:
      owner: "docs team"
```

**Singular data tests**

Add `meta` in the SQL test file using `config()`:

tests/my\_singular\_test.sql

```sql
{{ config(meta={'owner': 'docs team'}) }}

select * from {{ ref('my_model') }}
where my_column is null
```

Or document in `tests/properties.yml`:

tests/properties.yml

```yaml
data_tests:
  - name: my_singular_test
    config:
      meta:
        owner: "analytics_team"
```

💡Did you know\...

Available from dbt v

<!-- -->

1.8

<!-- -->

or with the

<!-- -->

[dbt "Latest" release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md).

dbt\_project.yml

```yml
unit_tests:
  <resource-path>:
    +meta: {<dictionary>}
```

models/\<filename>.yml

```yml
unit_tests:
  - name: <test-name>
    config:
      meta: {<dictionary>}
```

The `meta` config is not currently supported for analyses.

dbt\_project.yml

```yml
macros:
  <resource-path>:
    +meta: {<dictionary>}
```

macros/schema.yml

```yml
macros:
  - name: macro_name
    config: 
      meta: {<dictionary>} # changed to config in v1.11
    arguments:
      - name: argument_name
```

dbt\_project.yml

```yml
exposures:
  <resource-path>:
    +meta: {<dictionary>}
```

models/exposures.yml

```yml

exposures:
  - name: exposure_name
    config:
      meta: {<dictionary>} # changed to config in v1.10
```

Configure `meta` in the [semantic models](https://docs.getdbt.com/docs/build/semantic-models.md) embedded within your model YAML file or under the `semantic-models` config block in the `dbt_project.yml` file.

dbt\_project.yml

```yml
semantic-models:
  <resource-path>:
    +meta: {<dictionary>}
```

models/file\_name.yml

```yml
models:
  - name: model_name
    semantic_model:
      enabled: true
      config:
        meta: {<dictionary>}
```

[Dimensions](https://docs.getdbt.com/docs/build/dimensions.md), [entities](https://docs.getdbt.com/docs/build/entities.md), and metrics can also have their own `meta` configurations.

models/file\_name.yml

```yml
models:
  - name: model_name
    semantic_model:
      enabled: true
      config:
        meta: {<dictionary>}

    agg_time_dimension: your_time_dimension_name

    columns:
      - name: entity_column_name
        entity:
          type: primary
          name: entity_name
          config:
            meta: {<dictionary>}

      - name: dimension_column_name
        dimension:
          type: categorical
          name: dimension_name
          config:
            meta: {<dictionary>}

    metrics:
      - name: simple_metric_name
        description: "Description of the metric"
        type: simple
        agg: sum  
        expr: column_name 
        config:
          meta: {<dictionary>}
```

The `meta` config can be defined:

* Under the `semantic-models` config in the properties file (as showin in previous `models/semantic_models.yml` example)
* Under the `semantic-models` config in the project file (`dbt_project.yml`). See [configs and properties](https://docs.getdbt.com/reference/configs-and-properties.md) for details.

dbt\_project.yml

```yml
metrics:
  <resource-path>:
    +meta: {<dictionary>}
```

models/file\_name.yml

```yml
models:
  - name: model_name 
    semantic_model:
      enabled: true
    agg_time_dimension: your_time_dimension
    columns:
      - name: column_name
        dimension:
          type: time
        granularity: day
    metrics:
      - name: number_of_people
        type: simple
        description: Total count of people
        agg: count
        expr: people
        config:
          meta:
            my_meta_config: 'config_value'
```

dbt\_project.yml

```yml
saved-queries:
  <resource-path>:
    +meta: {<dictionary>}
```

models/semantic\_models.yml

```yml
saved_queries:
  - name: saved_query_name
    config:
      meta: {<dictionary>}
```

## Definition[​](#definition "Direct link to Definition")

The `meta` config sets metadata for a resource and accepts any key-value pairs. This metadata is compiled into the `manifest.json` file generated by dbt, and is visible in the auto-generated documentation.

Depending on the resource you're configuring, `meta` may be available within the `config` property, and/or as a top-level key. (For backwards compatibility, `meta` is often (but not always) supported as a top-level key, though without the capabilities of config inheritance.)

## Examples[​](#examples "Direct link to Examples")

To demonstrate how to use the `meta` config, here are some examples:

* [Designate a model owner](#designate-a-model-owner)
* [Designate a source column as containing PII](#designate-a-source-column-as-containing-pii)
* [Configure one meta attribute for all seeds](#configure-one-meta-attribute-for-all-seeds)
* [Override one meta attribute for a single model](#override-one-meta-attribute-for-a-single-model)
* [Assign owner and favorite\_color in the dbt\_project.yml as a config property](#assign-owner-and-favorite_color-in-the-dbt_projectyml-as-a-config-property)
* [Assign meta to semantic model](#assign-meta-to-semantic-model)
* [Assign meta to dimensions, measures, entities](#assign-meta-to-dimensions-measures-entities)
* [Add meta to generic and singular data tests](#add-meta-to-generic-and-singular-data-tests)
* [Access meta values in Python models](#access-meta-values-in-python-models)

### Designate a model owner[​](#designate-a-model-owner "Direct link to Designate a model owner")

Additionally, indicate the maturity of a model using a `model_maturity:` key.

models/schema.yml

```yml

models:
  - name: users
    config:
      meta:
        owner: "@alice"
        model_maturity: in dev
```

### Designate a source column as containing PII[​](#designate-a-source-column-as-containing-pii "Direct link to Designate a source column as containing PII")

models/schema.yml

```yml

sources:
  - name: salesforce
    tables:
      - name: account
        config:
          meta:
            contains_pii: true
        columns:
          - name: email
            config:
              meta: # changed to config in v1.10 and backported to 1.9
                contains_pii: true
```

### Configure one meta attribute for all seeds[​](#configure-one-meta-attribute-for-all-seeds "Direct link to Configure one meta attribute for all seeds")

dbt\_project.yml

```yml
seeds:
  +meta:
    favorite_color: red
```

### Override one meta attribute for a single model[​](#override-one-meta-attribute-for-a-single-model "Direct link to Override one meta attribute for a single model")

models/my\_model.sql

```sql
{{ config(meta = {
    'single_key': 'override'
}) }}

select 1 as id
```

<br />

### Assign owner and favorite\_color in the dbt\_project.yml as a config property[​](#assign-owner-and-favorite_color-in-the-dbt_projectyml-as-a-config-property "Direct link to Assign owner and favorite_color in the dbt_project.yml as a config property")

dbt\_project.yml

```yml
models:
  jaffle_shop:
    +meta:
      owner: "@alice"
      favorite_color: red
```

### Assign meta to semantic model[​](#assign-meta-to-semantic-model "Direct link to Assign meta to semantic model")

<!-- -->

The following example shows how to assign a `meta` value to a [semantic model](https://docs.getdbt.com/docs/build/semantic-models.md) in the model YAML file and `dbt_project.yml` file:

* Semantic model
* dbt\_project.yml

```yaml
models:
  - name: fact_transactions
    description: "Transaction fact table at the transaction level. This table contains one row per transaction and includes the transaction timestamp."
    semantic_model:
      enabled: true
      name: transaction
      config:
        meta:
          data_owner: "Finance team"
          used_in_reporting: true

    agg_time_dimension: transaction_date
```

```yaml
semantic-models:
  jaffle_shop:
    +meta:
      used_in_reporting: true
```

### Assign meta to dimensions, measures, entities[​](#assign-meta-to-dimensions-measures-entities "Direct link to Assign meta to dimensions, measures, entities")

<!-- -->

* Semantic model
* dbt\_project.yml

The following example shows how to assign a `meta` value to a [dimension](https://docs.getdbt.com/docs/build/dimensions.md), [entity](https://docs.getdbt.com/docs/build/entities.md), and [simple metrics](https://docs.getdbt.com/docs/build/simple.md) in a semantic model:

model\_name.yml

```yml
models:
  - name: model_name
    semantic_model:
      enabled: true
      name: semantic_model

    agg_time_dimension: order_date

    columns:
      - name: order_date
        dimension:
          type: time
          config:
            meta:
              data_owner: "Finance team"
              used_in_reporting: true

      - name: customer_id
        entity:
          type: primary
          config:
            meta:
              description: "Unique identifier for customers"
              data_owner: "Sales team"
              used_in_reporting: false

    metrics:
      - name: count_of_users
        type: simple
        agg: count_distinct
        expr: user_id
        config:
          meta:
            used_in_reporting: true
```

This second example shows how to assign a `data_owner` and additional metadata value to a dimension in the `dbt_project.yml` file using the `+meta` syntax. The similar syntax can be used for entities and simple metrics.

dbt\_project.yml

```yml
semantic-models:
  jaffle_shop:
    ...
    dimensions:
      - name: order_date
        config:
          meta:
            data_owner: "Finance team"
            used_in_reporting: true
```

### Add meta to generic and singular data tests[​](#add-meta-to-generic-and-singular-data-tests "Direct link to Add meta to generic and singular data tests")

The following examples show how to add `meta` to [generic data tests](https://docs.getdbt.com/docs/build/data-tests.md#generic-data-tests) in a `properties.yml` file, and to [singular data tests](https://docs.getdbt.com/docs/build/data-tests.md#singular-data-tests) using `config()`. You can also set defaults in `dbt_project.yml` or `tests/properties.yml`.

* Generic data test
* Singular data test

models/properties.yml

```yaml
models:
  - name: orders
    columns:
      - name: order_id
        data_tests:
          - not_null:
              config:
                meta:
                  owner: "@data_team"
```

tests/assert\_order\_ids.sql

```sql
{{ config(meta={'owner': '@data_team'}) }}

select *
from {{ ref('orders') }}
where order_id is null
```

### Access meta values in Python models[​](#access-meta-values-in-python-models "Direct link to Access meta values in Python models")

To access custom `meta` values in [Python models](https://docs.getdbt.com/docs/build/python-models.md), use the `dbt.config.meta_get()` method.

For example, if you have a model named `my_python_model` and you want to store custom values, you can do the following:

models/schema.yml

```yml
models:
  - name: my_python_model
    config:
      meta:
        batch_size: 1000
        processing_mode: "incremental"
```

models/my\_python\_model.py

```python
def model(dbt, session):
    # Access custom values stored in meta directly
    batch_size = dbt.config.meta_get("batch_size")
    processing_mode = dbt.config.meta_get("processing_mode")
    
    # Use the meta values in your model logic
    df = dbt.ref("upstream_model")
    
    if processing_mode == "incremental":
        df = df.limit(batch_size)
    
    return df
```
