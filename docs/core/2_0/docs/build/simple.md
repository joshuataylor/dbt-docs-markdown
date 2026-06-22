# Simple metrics

Simple metrics are metrics that directly reference a single column expression within a semantic model, without any additional columns involved. They are aggregations over a column in your data platform and can be filtered by one or multiple dimensions.

The parameters, description, and type for simple metrics are:

| Parameter                | Description                                                                                                                                                                                   | Required | Type    |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------- |
| `name`                   | The name of the metric. It must be unique within your project and can include lowercase letters, numbers, and underscores. Use this name to reference the metric from the Semantic Layer API. | Required | String  |
| `description`            | The description of the metric.                                                                                                                                                                | Optional | String  |
| `type`                   | The type of the metric (cumulative, derived, ratio, or simple).                                                                                                                               | Required | String  |
| `label`                  | Defines the display value in downstream tools. Accepts plain text, spaces, and quotes (such as `orders_total` or `"orders_total"`).                                                           | Required | String  |
| `agg`                    | The aggregation function to use. Use `sum`, `max`, `min`, `average`, `median`, `count`, `count_distinct`, `percentile`, and `sum_boolean`.                                                    | Required | String  |
| `expr`                   | The expression to use, like a column name. Defaults to the metric name.                                                                                                                       | Optional | String  |
| `percentile`             | The percentile to use. Required if `agg` is `percentile`.                                                                                                                                     | Optional | Integer |
| `percentile_type`        | The percentile type to use. Use `discrete` or `continuous`. Required for `percentile` metrics.                                                                                                | Optional | String  |
| `non_additive_dimension` | The non-additive dimension to use.                                                                                                                                                            | Optional | String  |
| `agg_time_dimension`     | The time dimension to use.                                                                                                                                                                    | Optional | String  |
| `join_to_timespine`      | Indicates if the aggregated measure should be joined to the time spine table to fill in missing dates. Default `false`.                                                                       | Optional | Boolean |
| `fill_nulls_with`        | Set the value in your metric definition instead of null (such as zero).                                                                                                                       | Optional | Integer |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

The following displays the complete specification for simple metrics, along with an example.

<!-- -->

Note that you must define simple metrics within a semantic model's YAML entry.

```yaml
models: 
  - name: my_model
    semantic_model: 
      enabled: true
     ...
    metrics:
      - name: my_simple_metric # Required
        description: The metric description # Optional
        label: My simple metric label # Optional
        type: simple  # Required
        agg: count_distinct # Required sum | max | min | average | median | count_distinct | percentile, and sum_boolean
        expr: case when is_a then 1 else 0 end # Optional for simple metric, defaults to name of metric
        join_to_timespine: true
        fill_nulls_with: 0

      - name: my_simple_metric_that_uses_the_other_time_dimension
        description: The metric description
        label: My simple metric that uses the other time dimension label
        type: simple
        agg: count_distinct
        agg_time_dimension: my_other_time_dimension_column # Optional, if not using the default time dimension
```

For advanced data modeling, you can use `fill_nulls_with` and `join_to_timespine` to [set null metric values to zero](https://docs.getdbt.com/docs/build/fill-nulls-advanced.md), ensuring numeric values for every data row.

## Simple metrics example[​](#simple-metrics-example "Direct link to Simple metrics example")

<!-- -->

```yaml
  metrics:
    - name: customers
      description: Count of customers
      type: simple
      label: Count of customers
      agg: count
      expr: customers 
      fill_nulls_with: 0
      join_to_timespine: true
      filter: "{{ Dimension('customer__customer_total') }} >= 20"
      # alias is not supported on simple metrics 
      # use alias in input_metrics when referencing this metric in derived, ratio, or conversion metrics

    - name: large_orders
      description: "Order with order values over 20."
      type: simple
      label: Large orders
      agg: count
      expr: orders 
      filter: "{{ Dimension('customer__order_total_dim') }} >= 20"
```

## Related docs[​](#related-docs "Direct link to Related docs")

* [Fill null values for simple, derived, or ratio metrics](https://docs.getdbt.com/docs/build/fill-nulls-advanced.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
