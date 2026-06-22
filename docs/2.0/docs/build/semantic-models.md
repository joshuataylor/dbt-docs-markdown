# Semantic models

Tip

Use [dbt Wizard](https://docs.getdbt.com/docs/platform/wizard-platform.md), available for dbt Enterprise and Enterprise+ accounts, to generate

<!-- -->

semantic models

<!-- -->

in the Studio IDE only.

Semantic models are the foundation for data definition in MetricFlow, which powers the Semantic Layer:

<!-- -->

* Think of semantic models as nodes connected by entities in a semantic graph. You can configure this graph by making metadata annotations on your dbt models to describe their usage in metric calculations.
* MetricFlow uses YAML configuration files to create this graph for querying metrics.
* Each semantic model corresponds to a dbt model in your DAG, requiring a unique YAML configuration for each semantic model.
* Each dbt model can define one semantic model via a `semantic_model` block. Use the optional `name` field if you need a different display name.
* Configure semantic models in a YAML file within your dbt project directory, embedded within your model definitions rather than as separate configurations.
* You can also define semantic models using [Open Semantic Interchange (OSI)](https://github.com/open-semantic-interchange/OSI) documents, an alternative to dbt Core's native YAML configuration. For more information, refer to [OSI semantic layer documents](https://docs.getdbt.com/docs/build/osi-semantic-models.md).

<!-- -->

📹 Learn about the dbt Semantic Layer with on-demand video courses!

Explore our [dbt Semantic Layer on-demand course](https://learn.getdbt.com/courses/semantic-layer) to learn how to define and query metrics in your dbt project.

Additionally, dive into mini-courses for querying the dbt Semantic Layer in your favorite tools: [Tableau](https://courses.getdbt.com/courses/tableau-querying-the-semantic-layer), [Excel](https://learn.getdbt.com/courses/querying-the-semantic-layer-with-excel), [Hex](https://courses.getdbt.com/courses/hex-querying-the-semantic-layer), and [Mode](https://courses.getdbt.com/courses/mode-querying-the-semantic-layer).

Here we describe the Semantic model components with examples:

<!-- -->

| Component                                                          | Description                                                                                                                                                                                                                                                                                                                                                                            | Required | Type   |
| ------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------ |
| [Name](#name)                                                      | Choose a name for the semantic model. Avoid using double underscores (\_\_) in the name as they're not supported. Defaults to value of the model name.                                                                                                                                                                                                                                 | Optional | String |
| [Time dimension](#time-dimension)                                  | Only `agg_time_dimension` is supported.                                                                                                                                                                                                                                                                                                                                                | Required | Dict   |
| [Entities](#entities)                                              | Define entities at the column level. For any column that serves as a join key, add an entity block under the column with `type` set to primary, foreign, unique, or natural. Optionally include name, description, label, config, or use expr when the join key differs from the column.                                                                                               | Required | List   |
| [Primary Entity](#primary-entity)                                  | If a column declares an entity with `type: primary`, you don’t need `primary_entity`. If no column is primary, set the top-level `primary_entity` to name the model’s primary entity.                                                                                                                                                                                                  | Optional | String |
| [Dimensions](#dimensions)                                          | Different ways to group or slice data for a metric, they can be `time` or `categorical`.                                                                                                                                                                                                                                                                                               | Required | List   |
| [Derived semantics](#derived_semantics-in-dimensions-and-entities) | Use to create dimensions and entities that don’t match a single, physical column. The `expr` field is required.                                                                                                                                                                                                                                                                        | Optional | List   |
| [Simple metrics](#measures)                                        | Aggregations applied to columns in your data model. They can be the final metric or used as building blocks for more complex metrics.                                                                                                                                                                                                                                                  | Optional | List   |
| [Label](#label)                                                    | The display name for your semantic model `node`, `dimension`, `entity`, and/or simple metrics.                                                                                                                                                                                                                                                                                         | Optional | String |
| `config`                                                           | Use the [`config`](https://docs.getdbt.com/reference/resource-properties/config.md) property to specify configurations for your metric. Supports [`meta`](https://docs.getdbt.com/reference/resource-configs/meta.md), [`group`](https://docs.getdbt.com/reference/resource-configs/group.md), and [`enabled`](https://docs.getdbt.com/reference/resource-configs/enabled.md) configs. | Optional | Dict   |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Semantic models components[​](#semantic-models-components "Direct link to Semantic models components")

The complete spec for semantic models is below:

<!-- -->

```yaml
models:
  - name: the_name_of_the_model # Model name
    semantic_model:
      enabled: true
      name: the_name_of_the_semantic_model # Optional: semantic model name, defaults to model name

    agg_time_dimension: dimension_name # Required

    # Entities, dimensions defined at column level
    columns:
      - name: entity_column_name
        entity:
          type: primary # or foreign
          name: entity_name
          description: entity description

      - name: dimension_column_name
        dimension:
          type: categorical # or time
          name: dimension_name
          description: dimension description

    # Simple metrics replace measures
    metrics:
      - name: metric_name
        description: metric description
        type: simple
        label: Metric Label
        agg: sum # Required
        expr: column_name # Optional, defaults to metric name
        # Other properties like fill_nulls_with, join_to_timespine can go here
```

The following example displays a complete configuration and detailed descriptions of each field:

<!-- -->

```yaml
models:
  - name: fact_transactions # Model name
    description: "Transaction fact table at the transaction level. This table contains one row per transaction and includes the transaction timestamp."
    semantic_model:
      enabled: true

    agg_time_dimension: transaction_date

    columns:
      - name: transaction_id
        entity: # Entities included in the table are defined here. MetricFlow will use these columns as join keys.
          type: primary
          name: transaction

      - name: customer_id
        entity:
          type: foreign
          name: customer

      - name: transaction_date
        granularity: day
        dimension: # Dimensions are qualitative values such as names, dates, or geographical data. They provide context to metrics and allow "metric by group" data slicing.
          type: time

      - name: order_country
        dimension:
          type: categorical
          name: transaction_location

    metrics: # Simple metrics are columns we perform an aggregation over. These are inputs to metrics.
      - name: transaction_total
        description: "The total value of the transaction."
        type: simple
        label: Transaction Total
        agg: sum
        expr: transaction_total

      - name: average_transaction_total
        description: "The average total sale of the transaction."
        type: simple
        label: Average Transaction Total
        agg: average
        expr: average_transaction_total

      - name: median_sales
        description: "The median sale of the transaction."
        type: simple
        label: Median Sales
        agg: median
        expr: transaction_total

  - name: dim_customers # Another model
    description: "A customer dimension table."
    semantic_model:
      enabled: true

    columns:
      - name: customer_id
        entity:
          type: primary
          name: customer

      - name: first_name
        dimension:
          type: categorical
          name: first_name
```

<!-- -->

Semantic models support [`meta`](https://docs.getdbt.com/reference/resource-configs/meta.md), [`group`](https://docs.getdbt.com/reference/resource-configs/group.md), and [`enabled`](https://docs.getdbt.com/reference/resource-configs/enabled.md) [`config`](https://docs.getdbt.com/reference/resource-properties/config.md) property in the schema file:

```yml
models:
  - name: orders
    semantic_model:
    enabled: true | false
      group: some_group
      config:
        meta:
          some_key: some_value
```

<!-- -->

### Name[​](#name-1 "Direct link to Name")

Define the name of the semantic model. If not provided, this defaults to the value of the model name. Avoid using double underscores (\_\_) in the name as they're not supported.

<!-- -->

<!-- -->

### Time dimension[​](#time-dimension "Direct link to Time dimension")

`agg_time_dimension` represents the default time dimensions for simple metrics. This can be overridden by adding the `agg_time_dimension` key directly to a simple metric - see [Dimensions](https://docs.getdbt.com/docs/build/dimensions.md) for examples.

<!-- -->

### Entities[​](#entities-1 "Direct link to Entities")

To specify the [entities](https://docs.getdbt.com/docs/build/entities.md) in your model, add an `entity` block at the column level with `type` set to primary, foreign, unique, or natural. Optionally, provide `name` and `expr` when the join key’s name differs from the column.

### Primary entity[​](#primary-entity "Direct link to Primary entity")

<!-- -->

MetricFlow requires that all dimensions be tied to an entity to guarantee unique dimension names. If your data source doesn't have a primary entity, you need to assign the entity under the column that serves as the key by adding `type: primary`. If no column is marked as primary, set a top-level `primary_entity` to name the model’s primary entity. The primary entity doesn't necessarily have to map to a column in that table and assigning the name doesn't affect query generation.

You can define a primary entity using the following configs:

<!-- -->

```yaml
models:
  - name: bookings_monthly_source
    semantic_model:
      enabled: true

    agg_time_dimension: ds

    columns:
      - name: booking_id
        entity:
          type: primary
          name: booking_id

      - name: ds
        dimension:
          type: time
          name: ds

    metrics:
      - name: bookings_monthly
        description: "Sum of bookings monthly"
        type: simple
        label: "Bookings Monthly"
        agg: sum
        expr: bookings_monthly
```

* Entity types
* Sample config

Here are the types of keys:

* **Primary** — Only one record per row in the table, and it includes every record in the data platform.
* **Unique** — Only one record per row in the table, but it may have a subset of records in the data platform. Null values may also be present.
* **Foreign** — Can have zero, one, or multiple instances of the same record. Null values may also be present.
* **Natural** — A column or combination of columns in a table that uniquely identifies a record based on real-world data. For example, the `sales_person_id` can serve as a natural key in a `sales_person_department` dimension table.

This example shows a semantic model with three entities and their entity types: `transaction` (primary), `order` (foreign), and `user` (foreign).

To reference a desired column, use the actual column name from the model in the `name` parameter. You can also use `name` as an alias to rename the column, and the `expr` parameter to refer to the original column name or a SQL expression of the column.

```yaml
entity:
  - name: transaction
    type: primary
  - name: order
    type: foreign
    expr: id_order
  - name: user
    type: foreign
    expr: substring(id_order FROM 2)
```

You can refer to entities (join keys) in a semantic model using the `name` parameter. Entity names must be unique within a semantic model, and identifier names can be non-unique across semantic models since MetricFlow uses them for [joins](https://docs.getdbt.com/docs/build/join-logic.md).

### Dimensions[​](#dimensions "Direct link to Dimensions")

[Dimensions](https://docs.getdbt.com/docs/build/dimensions.md) are different ways to organize or look at data. They are effectively the group by parameters for metrics. For example, you might group data by things like region, country, or job title.

<!-- -->

MetricFlow takes a dynamic approach when making dimensions available for metrics. Instead of trying to figure out all the possible groupings ahead of time, MetricFlow lets you ask for the dimensions you need and constructs any joins necessary to reach the requested dimensions at query time. The advantage of this approach is that you don't need to set up a system that pre-materializes every possible way to group data, which can be time-consuming and prone to errors. Instead, you define dimensions within the semantic model by nesting them under columns, and they will automatically be made available for valid metrics.

Dimensions have the following characteristics:

* There are two types of dimensions: categorical and time. Categorical dimensions are for things you can't measure in numbers, while time dimensions represent dates and timestamps. Time dimensions require a column-level granularity.
* Dimensions are bound to the primary entity of the semantic model in which they are defined. For example, if a dimension called `full_name` is defined in a model with `user` as a primary entity, then `full_name` is scoped to the `user` entity. To reference this dimension, you would use the fully qualified dimension name `user__full_name`.
* The naming of dimensions must be unique in each semantic model with the same primary entity. Dimension names can be repeated if defined in semantic models with a different primary entity.

For time groups

For semantic models that define metrics, include at least one time dimension column with a `granularity` and set the model’s `agg_time_dimension`. You can override the aggregation time dimension on individual metrics if needed.

<!-- -->

### `derived_semantics` in dimensions and entities[​](#derived_semantics-in-dimensions-and-entities "Direct link to derived_semantics-in-dimensions-and-entities")

Use the `derived_semantics` key in the model YAML entry when you want to create dimensions and entities that don’t match a single, physical column. The `expr` field is required when using `derived_semantics`. For more information, see [Dimensions](https://docs.getdbt.com/docs/build/dimensions.md#derived_semantics-in-dimensions) and [Entities](https://docs.getdbt.com/docs/build/entities.md#derived_semantics-in-entities).

### Simple metrics[​](#simple-metrics "Direct link to Simple metrics")

Simple metrics are direct aggregations over columns in your data warehouse using different aggregation types. They serve as building blocks for more complex metrics and can be filtered by dimensions.

Simple metrics have various parameters which are listed in a table along with their descriptions and types. For more information, see [Simple metrics](https://docs.getdbt.com/docs/build/simple.md).

<!-- -->

## Dependencies[​](#dependencies "Direct link to Dependencies")

<!-- -->

Metric nodes will reflect dependencies on semantic models based on their simple metrics. However, dependencies based on filters should not be reflected in:

* [dbt selection syntax](https://docs.getdbt.com/reference/node-selection/syntax.md)
* Visualization of the DAG in dbt-docs and the [integrated development environment](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md) (IDE).

This is because metrics need to source nodes for their `depends_on` attribute from a few different places:

* `RATIO` metrics depend on `numerator` and `denominator` metrics, while `DERIVED` metrics depend on `input_metrics`.
* `SIMPLE` type metrics depend on their semantic\_model.

For example, when you run the command `dbt list --select my_semantic_model+`, it will show you the metrics that belong to the specified semantic model.

But there's a condition: Only the metrics that actually use simple metrics or derived metrics from that semantic model will be included in the list. In other words, if a metric only uses a dimension from the semantic model in its filters, it won't be considered as part of that semantic model.

## Related docs[​](#related-docs "Direct link to Related docs")

<!-- -->

* [About MetricFlow](https://docs.getdbt.com/docs/build/about-metricflow.md)
* [Dimensions](https://docs.getdbt.com/docs/build/dimensions.md)
* [Entities](https://docs.getdbt.com/docs/build/entities.md)
* [Simple metrics](https://docs.getdbt.com/docs/build/simple.md)
* [Semantic Layer best practices guide](https://docs.getdbt.com/best-practices/how-we-build-our-metrics/semantic-layer-1-intro.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
