# Cumulative metrics

<!-- -->

## Parameters[​](#parameters "Direct link to Parameters")

<!-- -->

<!-- -->

### Complete specification[​](#complete-specification "Direct link to Complete specification")

The following displays the complete specification for cumulative metrics, along with an example:

<!-- -->

<!-- -->

## Cumulative metrics example[​](#cumulative-metrics-example "Direct link to Cumulative metrics example")

Cumulative metrics measure data over a given window and consider the window infinite when no window parameter is passed, accumulating the data over all time.

The following example shows how to define cumulative metrics in a YAML file:

<!-- -->

<!-- -->

<!-- -->

### Granularity options[​](#granularity-options "Direct link to Granularity options")

<!-- -->

<!-- -->

 Expand toggle to view how the SQL compiles

Note the use of the `window` function to select the `first` value. For `last` and `average`, we would replace the `first_value()` function in the generated SQL with `last_value()` and `average` respectively.

```sql
-- re-aggregate metric via the group by
select
  metric_time__week,
  metric_time__quarter,
  revenue_all_time
from (
  -- window function for metric re-aggregation
  select
    metric_time__week,
    metric_time__quarter,
    first_value(revenue_all_time) over (
      partition by
        metric_time__week,
        metric_time__quarter
      order by metric_time__day
      rows between unbounded preceding and unbounded following
    ) as revenue_all_time
  from (
    -- join self over time range
    -- pass only elements: ['txn_revenue', 'metric_time__week', 'metric_time__quarter', 'metric_time__day']
    -- aggregate measures
    -- compute metrics via expressions
    select
      subq_11.metric_time__day as metric_time__day,
      subq_11.metric_time__week as metric_time__week,
      subq_11.metric_time__quarter as metric_time__quarter,
      sum(revenue_src_28000.revenue) as revenue_all_time
    from (
      -- time spine
      select
        ds as metric_time__day,
        date_trunc('week', ds) as metric_time__week,
        date_trunc('quarter', ds) as metric_time__quarter
      from mf_time_spine subq_12
      group by
        ds,
        date_trunc('week', ds),
        date_trunc('quarter', ds)
    ) subq_11
    inner join fct_revenue revenue_src_28000
    on (
      date_trunc('day', revenue_src_28000.created_at) <= subq_11.metric_time__day
    )
    group by
      subq_11.metric_time__day,
      subq_11.metric_time__week,
      subq_11.metric_time__quarter
  ) subq_16
) subq_17
group by
  metric_time__week,
  metric_time__quarter,
  revenue_all_time
```

### Window options[​](#window-options "Direct link to Window options")

This section details examples of when to specify and not to specify window options.

<!-- -->

<!-- -->

### Grain to date[​](#grain-to-date "Direct link to Grain to date")

You can choose to specify a grain to date in your cumulative metric configuration to accumulate a metric from the start of a grain (such as week, month, or year). When using a window, such as a month, MetricFlow will go back one full calendar month. However, grain to date will always start accumulating from the beginning of the grain, regardless of the latest date of data.

<!-- -->

<!-- -->

We can compare the difference between a 1-month window and a monthly grain to date.

* The cumulative metric in a window approach applies a sliding window of 1 month
* The grain to date by month resets at the beginning of each month.

<!-- -->

<!-- -->

Cumulative metric with grain to date:

<!-- -->

<!-- -->

 Expand toggle to view how the SQL compiles

```sql
with staging as (
    select
        subq_3.date_day as metric_time__day,
        date_trunc('week', subq_3.date_day) as metric_time__week,
        sum(subq_1.order_count) as orders_last_month_to_date
    from dbt_jstein.metricflow_time_spine subq_3
    inner join (
        select
            date_trunc('day', ordered_at) as metric_time__day,
            1 as order_count
        from analytics.dbt_jstein.orders orders_src_10000
    ) subq_1
    on (
        subq_1.metric_time__day <= subq_3.date_day
    ) and (
        subq_1.metric_time__day >= date_trunc('month', subq_3.date_day)
    )
    group by
        subq_3.date_day,
        date_trunc('week', subq_3.date_day)
)

select
    *
from (
    select
        metric_time__week,
        first_value(orders_last_month_to_date) over (partition by date_trunc('week', metric_time__day) order by metric_time__day) as cumulative_revenue
    from
        staging
)
group by
    metric_time__week,
    cumulative_revenue
order by
    metric_time__week
    1
```

## SQL implementation example[​](#sql-implementation-example "Direct link to SQL implementation example")

To calculate the cumulative value of the metric over a given window we do a time range join to a timespine table using the primary time dimension as the join key. We use the accumulation window in the join to decide whether a record should be included on a particular day. The following SQL code produced from an example cumulative metric is provided for reference:

To implement cumulative metrics, refer to the SQL code example:

```sql
select
  count(distinct distinct_users) as weekly_active_users,
  metric_time
from (
  select
    subq_3.distinct_users as distinct_users,
    subq_3.metric_time as metric_time
  from (
    select
      subq_2.distinct_users as distinct_users,
      subq_1.metric_time as metric_time
    from (
      select
        metric_time
      from transform_prod_schema.mf_time_spine subq_1356
      where (
        metric_time >= cast('2000-01-01' as timestamp)
      ) and (
        metric_time <= cast('2040-12-31' as timestamp)
      )
    ) subq_1
    inner join (
      select
        distinct_users as distinct_users,
        date_trunc('day', ds) as metric_time
      from demo_schema.transactions transactions_src_426
      where (
        (date_trunc('day', ds)) >= cast('1999-12-26' as timestamp)
      ) AND (
        (date_trunc('day', ds)) <= cast('2040-12-31' as timestamp)
      )
    ) subq_2
    on
      (
        subq_2.metric_time <= subq_1.metric_time
      ) and (
        subq_2.metric_time > dateadd(day, -7, subq_1.metric_time)
      )
  ) subq_3
)
group by
  metric_time,
limit 100;
```

## Limitations[​](#limitations "Direct link to Limitations")

If you specify a `window` in your cumulative metric definition, you must include `metric_time` as a dimension in the SQL query. This is because the accumulation window is based on metric time. For example,

```sql
select
  count(distinct subq_3.distinct_users) as weekly_active_users,
  subq_3.metric_time
from (
  select
    subq_2.distinct_users as distinct_users,
    subq_1.metric_time as metric_time
group by
  subq_3.metric_time
```

## Related docs[​](#related-docs "Direct link to Related docs")

* [Fill null values for simple, derived, or ratio metrics](https://docs.getdbt.com/docs/build/fill-nulls-advanced.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
