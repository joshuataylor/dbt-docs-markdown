# Ratio metrics

Ratio metrics allow you to create a ratio between two metrics. You specify a numerator and a denominator metric. You can optionally apply filters, names, and aliases to both the numerator and denominator when computing the metric.

The parameters for ratio metrics are as follows:

<!-- -->

<!-- -->

The complete specification for ratio metrics is as follows:

<!-- -->

<!-- -->

For advanced data modeling, you can use `fill_nulls_with` and `join_to_timespine` to [set null metric values to zero](https://docs.getdbt.com/docs/build/fill-nulls-advanced.md), ensuring numeric values for every data row.

## Ratio metrics example[​](#ratio-metrics-example "Direct link to Ratio metrics example")

These examples demonstrate how to create ratio metrics in your model. They cover basic and advanced use cases, including applying filters to the numerator and denominator metrics.

#### Example 1[​](#example-1 "Direct link to Example 1")

This example is a basic ratio metric that calculates the ratio of food orders to total orders:

<!-- -->

<!-- -->

#### Example 2[​](#example-2 "Direct link to Example 2")

This example is a ratio metric that calculates the ratio of food orders to total orders, with a filter and alias applied to the numerator. Note that in order to add these attributes, you'll need to use an explicit key for the name attribute too.

<!-- -->

<!-- -->

## Ratio metrics using different semantic models[​](#ratio-metrics-using-different-semantic-models "Direct link to Ratio metrics using different semantic models")

The system will simplify and turn the numerator and denominator into a ratio metric from different semantic models by computing their values in sub-queries. It will then join the result set based on common dimensions to calculate the final ratio. Here's an example of the SQL generated for such a ratio metric.

```sql
select
  subq_15577.metric_time as metric_time,
  cast(subq_15577.mql_queries_created_test as double) / cast(nullif(subq_15582.distinct_query_users, 0) as double) as mql_queries_per_active_user
from (
  select
    metric_time,
    sum(mql_queries_created_test) as mql_queries_created_test
  from (
    select
      cast(query_created_at as date) as metric_time,
      case when query_status in ('PENDING','MODE') then 1 else 0 end as mql_queries_created_test
    from prod_dbt.mql_query_base mql_queries_test_src_2552 
  ) subq_15576
  group by
    metric_time
) subq_15577
inner join (
  select
    metric_time,
    count(distinct distinct_query_users) as distinct_query_users
  from (
    select
      cast(query_created_at as date) as metric_time,
      case when query_status in ('MODE','PENDING') then email else null end as distinct_query_users
    from prod_dbt.mql_query_base mql_queries_src_2585 
  ) subq_15581
  group by
    metric_time
) subq_15582
on
  (
    (
      subq_15577.metric_time = subq_15582.metric_time
    ) or (
      (
        subq_15577.metric_time is null
      ) and (
        subq_15582.metric_time is null
      )
    )
  )
```

## Add filter[​](#add-filter "Direct link to Add filter")

Users can define constraints on input metrics for a ratio metric by applying a filter directly to the input metric, like so:

<!-- -->

<!-- -->

Note the `filter` and `alias` parameters for the metric referenced in the numerator.

* Use the `filter` parameter to apply a filter to the metric it's attached to.
* The `alias` parameter is used to avoid naming conflicts in the rendered SQL queries when the same metric is used with different filters.
* If there are no naming conflicts, the `alias` parameter can be left out.

## Related docs[​](#related-docs "Direct link to Related docs")

* [Fill null values for simple, derived, or ratio metrics](https://docs.getdbt.com/docs/build/fill-nulls-advanced.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
