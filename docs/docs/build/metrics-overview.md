# Creating metrics

After building [semantic models](https://docs.getdbt.com/docs/build/semantic-models.md), it's time to start adding metrics. This page explains the different supported metric types you can add to your dbt project.

<!-- -->

<!-- -->

## Parameters[​](#parameters "Direct link to Parameters")

The keys for metrics parameters are:

<!-- -->

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

<!-- -->

<!-- -->

<!-- -->

📹 Learn about the dbt Semantic Layer with on-demand video courses!

Explore our [dbt Semantic Layer on-demand course](https://learn.getdbt.com/courses/semantic-layer) to learn how to define and query metrics in your dbt project.

Additionally, dive into mini-courses for querying the dbt Semantic Layer in your favorite tools: [Tableau](https://courses.getdbt.com/courses/tableau-querying-the-semantic-layer), [Excel](https://learn.getdbt.com/courses/querying-the-semantic-layer-with-excel), [Hex](https://courses.getdbt.com/courses/hex-querying-the-semantic-layer), and [Mode](https://courses.getdbt.com/courses/mode-querying-the-semantic-layer).

## Default granularity for metrics[​](#default-granularity-for-metrics "Direct link to Default granularity for metrics")

<!-- -->

<!-- -->

## Conversion metrics[​](#conversion-metrics "Direct link to Conversion metrics")

[Conversion metrics](https://docs.getdbt.com/docs/build/conversion.md) help you track when a base event and a subsequent conversion event occur for an entity within a set time period.

<!-- -->

<!-- -->

## Cumulative metrics[​](#cumulative-metrics "Direct link to Cumulative metrics")

<!-- -->

<!-- -->

## Derived metrics[​](#derived-metrics "Direct link to Derived metrics")

[Derived metrics](https://docs.getdbt.com/docs/build/derived.md) allow you to perform calculations using other metrics. For example, you can calculate `gross_profit` by subtracting a `cost` metric from a `revenue` metric, or calculate growth by comparing a metric to its value from a previous time period.

<!-- -->

<!-- -->

## Ratio metrics[​](#ratio-metrics "Direct link to Ratio metrics")

[Ratio metrics](https://docs.getdbt.com/docs/build/ratio.md) involve a numerator metric and a denominator metric. A `filter` string can be applied to both the numerator and denominator or separately to the numerator or denominator.

<!-- -->

<!-- -->

## Simple metrics[​](#simple-metrics "Direct link to Simple metrics")

<!-- -->

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
