# Creating metrics

After building [semantic models](https://docs.getdbt.com/docs/build/semantic-models.md), it's time to start adding metrics. This page explains the different supported metric types you can add to your dbt project.

Metrics must be defined in a YAML file — either within the same file as your semantic models or in a separate YAML file in a subdirectory of your dbt project. They shouldn't be defined in a `config` block on a model.

<!-- -->

## Parameters[​](#parameters "Direct link to Parameters")

The keys for metrics parameters are:

| Parameter     | Description                                                                                                                                                                                                                                                                                                                                                                                   | Required | Type   |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------ |
| `name`        | Provide the reference name for the metric. This name must be a unique metric name and can consist of lowercase letters, numbers, and underscores.                                                                                                                                                                                                                                             | Required | String |
| `description` | Describe your metric.                                                                                                                                                                                                                                                                                                                                                                         | Optional | String |
| `type`        | Define the type of metric, which can be `conversion`, `cumulative`, `derived`, `ratio`, or `simple`.                                                                                                                                                                                                                                                                                          | Required | String |
| `type_params` | Additional parameters used to configure metrics. `type_params` are different for each metric type.                                                                                                                                                                                                                                                                                            | Required | Dict   |
| `label`       | Required string that defines the display value in downstream tools. Accepts plain text, spaces, and quotes (such as `orders_total` or `"orders_total"`).                                                                                                                                                                                                                                      | Required | String |
| `config`      | Use the [`config`](https://docs.getdbt.com/reference/resource-properties/config.md) property to specify configurations for your metric. Supports [`meta`](https://docs.getdbt.com/reference/resource-configs/meta.md), [`group`](https://docs.getdbt.com/reference/resource-configs/group.md), and [`enabled`](https://docs.getdbt.com/reference/resource-configs/enabled.md) configurations. | Optional | Dict   |
| `filter`      | You can optionally add a [filter](#filters) string to any metric type, applying filters to dimensions, entities, time dimensions, or other metrics during metric computation. Consider it as your WHERE clause.                                                                                                                                                                               | Optional | String |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

<!-- -->

### Type-specific parameters[​](#type-specific-parameters "Direct link to Type-specific parameters")

Each metric type has additional specific parameters:

* **Simple metrics**: `agg` (required), `expr`, `percentile`, `percentile_type`, `non_additive_dimension`, `agg_time_dimension`, `join_to_timespine`, `fill_nulls_with`
* **Cumulative metrics**: `input_metric` (required), `window`, `grain_to_date`, `period_agg`
* **Derived metrics**: `expr` (required), `input_metrics` (required)
* **Ratio metrics**: `numerator` (required), `denominator` (required)
* **Conversion metrics**: `entity` (required), `calculation` (required), `base_metric` (required), `conversion_metric` (required), `window`, `constant_properties`

Refer to the following sections about each metric type for detailed information on type-specific parameters.

### Example[​](#example "Direct link to Example")

Here's a complete example of the metrics spec configuration:

models/metrics/file\_name.yml

```yaml
metrics:
  - name: metric name                     ## Required
    description: description               ## Optional
    type: the type of the metric          ## Required
    type_params:                          ## Required
      - specific properties for the metric type
    config:                               ## Optional
      meta:
        my_meta_config:  'config'         ## Optional
    label: The display name for your metric. This value will be shown in downstream tools. ## Required
    filter: |                             ## Optional            
      {{  Dimension('entity__name') }} > 0 and {{ Dimension(' entity__another_name') }} is not
      null and {{ Metric('metric_name', group_by=['entity_name']) }} > 5
```

<!-- -->

<!-- -->

📹 Learn about the dbt Semantic Layer with on-demand video courses!

Explore our [dbt Semantic Layer on-demand course](https://learn.getdbt.com/courses/semantic-layer) to learn how to define and query metrics in your dbt project.

Additionally, dive into mini-courses for querying the dbt Semantic Layer in your favorite tools: [Tableau](https://courses.getdbt.com/courses/tableau-querying-the-semantic-layer), [Excel](https://learn.getdbt.com/courses/querying-the-semantic-layer-with-excel), [Hex](https://courses.getdbt.com/courses/hex-querying-the-semantic-layer), and [Mode](https://courses.getdbt.com/courses/mode-querying-the-semantic-layer).

## Default granularity for metrics[​](#default-granularity-for-metrics "Direct link to Default granularity for metrics")

It's possible to define a default time granularity for metrics if it's different from the granularity of the default aggregation time dimensions (`metric_time`). This is useful if your time dimension has a very fine grain, like second or hour, but you typically query metrics rolled up at a coarser grain.

The granularity can be set using the `time_granularity` parameter on the metric, and defaults to `day`. If day is not available because the dimension is defined at a coarser granularity, it will default to the defined granularity for the dimension.

### Example[​](#example-1 "Direct link to Example")

* You have a semantic model called `orders` with a time dimension called `order_time`.
* You want the `orders` metric to roll up to `monthly` by default; however, you want the option to look at these metrics hourly.
* You can set the `time_granularity` parameter on the `order_time` dimension to `hour`, and then set the `time_granularity` parameter in the metric to `month`.

```yaml
semantic_models:
  ...
  dimensions:
    - name: order_time
      type: time
      type_params:
      time_granularity: hour
  measures:
    - name: orders
      expr: 1
      agg: sum

metrics:
  - name: orders
    type: simple
    label: Count of Orders
    type_params:
      measure:
        name: orders
    time_granularity: month -- Optional, defaults to day
```

<!-- -->

## Conversion metrics[​](#conversion-metrics "Direct link to Conversion metrics")

[Conversion metrics](https://docs.getdbt.com/docs/build/conversion.md) help you track when a base event and a subsequent conversion event occur for an entity within a set time period.

models/metrics/file\_name.yml

```yaml
metrics:
  - name: The metric name 
    description: The metric description 
    type: conversion 
    label: YOUR_LABEL 
    type_params: #
      conversion_type_params: 
        entity: ENTITY
        calculation: CALCULATION_TYPE 
        base_measure: 
          name: The name of the measure 
          fill_nulls_with: Set the value in your metric definition instead of null (such as zero) 
          join_to_timespine: true/false
        conversion_measure:
          name: The name of the measure 
          fill_nulls_with: Set the value in your metric definition instead of null (such as zero) 
          join_to_timespine: true/false
        window: TIME_WINDOW
        constant_properties:
          - base_property: DIMENSION or ENTITY
            conversion_property: DIMENSION or ENTITY
```

<!-- -->

## Cumulative metrics[​](#cumulative-metrics "Direct link to Cumulative metrics")

[Cumulative metrics](https://docs.getdbt.com/docs/build/cumulative.md) aggregate a measure over a given window. If no window is specified, the window will accumulate the measure over all of the recorded time period. Note that you will need to create the [time spine model](https://docs.getdbt.com/docs/build/metricflow-time-spine.md) before you add cumulative metrics.

models/metrics/file\_name.yml

```yaml
# Cumulative metrics aggregate a measure over a given window. The window is considered infinite if no window parameter is passed (accumulate the measure over all of time)
metrics:
  - name: wau_rolling_7
    type: cumulative
    label: Weekly active users
    type_params:
      measure:
        name: active_users
        fill_nulls_with: 0
        join_to_timespine: true
      cumulative_type_params:
        window: 7 days
```

<!-- -->

## Derived metrics[​](#derived-metrics "Direct link to Derived metrics")

[Derived metrics](https://docs.getdbt.com/docs/build/derived.md) allow you to perform calculations using other metrics. For example, you can calculate `gross_profit` by subtracting a `cost` metric from a `revenue` metric, or calculate growth by comparing a metric to its value from a previous time period.

models/metrics/file\_name.yml

```yaml
metrics:
  - name: order_gross_profit
    description: Gross profit from each order.
    type: derived
    label: Order gross profit
    type_params:
      expr: revenue - cost
      metrics:
        - name: order_total
          alias: revenue
        - name: order_cost
          alias: cost
```

<!-- -->

## Ratio metrics[​](#ratio-metrics "Direct link to Ratio metrics")

[Ratio metrics](https://docs.getdbt.com/docs/build/ratio.md) involve a numerator metric and a denominator metric. A `filter` string can be applied to both the numerator and denominator or separately to the numerator or denominator.

models/metrics/file\_name.yml

```yaml
metrics:
  - name: cancellation_rate
    type: ratio
    label: Cancellation rate
    type_params:
      numerator: cancellations
      denominator: transaction_amount
    filter: |   
      {{ Dimension('customer__country') }} = 'MX'
  - name: enterprise_cancellation_rate
    type: ratio
    type_params:
      numerator:
        name: cancellations
        filter: {{ Dimension('company__tier') }} = 'enterprise'  
      denominator: transaction_amount
    filter: | 
      {{ Dimension('customer__country') }} = 'MX' 
```

<!-- -->

## Simple metrics[​](#simple-metrics "Direct link to Simple metrics")

[Simple metrics](https://docs.getdbt.com/docs/build/simple.md) point directly to a measure. You may think of it as a function that takes only one measure as the input.

* `name` — Use this parameter to define the reference name of the metric. The name must be unique amongst metrics and can include lowercase letters, numbers, and underscores. You can use this name to call the metric from the Semantic Layer API.

**Note:** If you've already defined the measure using the `create_metric: True` parameter, you don't need to create simple metrics. However, if you would like to include a constraint on top of the measure, you will need to create a simple type metric.

models/metrics/file\_name.yml

```yaml
metrics:
  - name: cancellations
    description: The number of cancellations
    type: simple
    label: Cancellations
    type_params:
      measure:
        name: cancellations_usd  # Specify the measure you are creating a proxy for.
        fill_nulls_with: 0
        join_to_timespine: true
    filter: |
      {{ Dimension('order__value')}} > 100 and {{Dimension('user__acquisition')}} is not null
```

<!-- -->

## Filters[​](#filters "Direct link to Filters")

Configure a filter using Jinja templating and the following syntax to reference entities, dimensions, time dimensions, or metrics in filters.

Refer to [Metrics as dimensions](https://docs.getdbt.com/docs/build/ref-metrics-in-filters.md) for details on how to use metrics as dimensions with metric filters:

models/metrics/file\_name.yml

```yaml
filter: |
  {{ Entity('entity_name') }}

filter: |
  {{ Dimension('primary_entity__dimension_name') }}

filter: |
  {{ TimeDimension('time_dimension', 'granularity') }}

filter: |
 {{ Metric('metric_name', group_by=['entity_name']) }}
```

For example, if you want to filter for the order date dimension grouped by month, use the following syntax:

```yaml
filter: |
  {{ TimeDimension('order_date', 'month') }}
```

## Related docs[​](#related-docs "Direct link to Related docs")

* [Semantic models](https://docs.getdbt.com/docs/build/semantic-models.md)
* [Fill null values for metrics](https://docs.getdbt.com/docs/build/fill-nulls-advanced.md)
* [Metrics as dimensions with metric filters](https://docs.getdbt.com/docs/build/ref-metrics-in-filters.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
