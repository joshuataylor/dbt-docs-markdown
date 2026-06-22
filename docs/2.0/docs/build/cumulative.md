# Cumulative metrics

Cumulative metrics aggregate values from other metrics across a defined accumulation period. If you don’t specify a period, the metric accumulates values over the entire available time range.

Use cumulative metrics when you want to calculate rolling or period-to-date values, such as weekly active users or month-to-date revenue.

* You must create a [time spine model](https://docs.getdbt.com/docs/build/metricflow-time-spine.md) before you define cumulative metrics so that MetricFlow can join time-based aggregations to the time spine.
* If a cumulative metric depends on metrics or dimensions defined in a different semantic model, set cumulative metrics under the top level `metrics` key.

<!-- -->

## Parameters[​](#parameters "Direct link to Parameters")

| Parameter             | Description                                                                                                                                                                                                                                                                      | Required | Type    |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------- |
| `name`                | The name of the metric.                                                                                                                                                                                                                                                          | Required | String  |
| `description`         | The description of the metric.                                                                                                                                                                                                                                                   | Optional | String  |
| `type`                | The type of the metric (cumulative, derived, ratio, or simple).                                                                                                                                                                                                                  | Required | String  |
| `label`               | Optional string that defines the display value in downstream tools. Accepts plain text, spaces, and quotes (such as `orders_total` or `"orders_total"`).                                                                                                                         | Optional | String  |
| `input_metric`        | The name of the metric being referenced. Supports the following nested parameters: `name`, `filter`, and `alias`.                                                                                                                                                                | Required | Dict    |
| `input_metric.name`   | The name of the metric being referenced.                                                                                                                                                                                                                                         | Required | String  |
| `input_metric.filter` | The [filter](https://docs.getdbt.com/docs/build/metrics-overview.md#filters) to apply to the metric.                                                                                                                                                                             | Optional | String  |
| `input_metric.alias`  | The alias to apply to the metric.                                                                                                                                                                                                                                                | Optional | String  |
| `join_to_timespine`   | Boolean indicating if the aggregated metric should be joined to the time spine table to fill in missing dates. Default is `false`.                                                                                                                                               | Optional | Boolean |
| `window`              | Specifies the accumulation window, such as `1 month`, `7 days`, or `1 year`. Cannot be used with `grain_to_date`.                                                                                                                                                                | Optional | String  |
| `grain_to_date`       | Sets the accumulation grain, such as `hour`, `day`, `week`, `month`, `year`, restarting accumulation at the beginning of each specified grain period. For example, selecting `month` will aggregate the `month to date` aggregation of the metric. Cannot be used with `window`. | Optional | String  |
| `period_agg`          | Defines how to re-aggregate the cumulative metric when querying with a non-default granularity: `first`, `last`, or `average`. Defaults to `first` if `period_agg` isn't specified.                                                                                              | Optional | String  |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

<!-- -->

### Complete specification[​](#complete-specification "Direct link to Complete specification")

The following displays the complete specification for cumulative metrics, along with an example:

<!-- -->

```yaml
metrics:
  - name: my_advanced_cumulative_metric # required 
    description: my_description # optional
    label: my_label # optional
    type: cumulative # required --  cumulative | ratio | derived | conversion
    # if cumulative, optionally can supply window or grain_to_date, but not both
    window: 1 week # (interval)
    # grain_to_date: hour # hour | day | week | month | year | ...
    input_metric: # required, must refer to metric name or metric dict
      name: my_metric_name_from_another_semantic_model
      filter: "{{ Dimension('entity__dimension_name') }} > 10"
      alias: my_metric_name_a_week_ago_in_another_semantic_model
```

## Cumulative metrics example[​](#cumulative-metrics-example "Direct link to Cumulative metrics example")

Cumulative metrics measure data over a given window and consider the window infinite when no window parameter is passed, accumulating the data over all time.

The following example shows how to define cumulative metrics in a YAML file:

<!-- -->

* `cumulative_order_total`: Calculates the cumulative order total over all time. Uses `input_metric` to specify the simple metric `order_total` to be aggregated.

* `cumulative_order_total_l1m`: Calculates the trailing 1-month cumulative order total. Uses `window` parameter to specify a window of 1 month.

* `cumulative_order_total_mtd`: Calculates the month-to-date cumulative order total. Uses `grain_to_date` parameter to specify a grain of `month`.

models/marts/fct\_orders.yml

```yaml
metrics:
  - name: cumulative_order_total
    label: "Cumulative order total (All-Time)"    
    description: "The cumulative value of all orders"
    type: cumulative
    input_metric: order_total
  
  - name: cumulative_order_total_l1m
    label: "Cumulative order total (L1M)"   
    description: "Trailing 1-month cumulative order total"
    type: cumulative
    window: 1 month
    input_metric: order_total
  
  - name: cumulative_order_total_mtd
    label: "Cumulative order total (MTD)"
    description: "The month-to-date value of all orders"
    type: cumulative
    grain_to_date: month
    input_metric: order_total
```

<!-- -->

### Granularity options[​](#granularity-options "Direct link to Granularity options")

<!-- -->

Use the `period_agg` parameter with `first`, `last`, and `average` functions to aggregate cumulative metrics over the requested period. This is because granularity options for cumulative metrics are different than the options for other metric types.

* For other metrics, we use the `date_trunc` function to implement granularity.
* However, cumulative metrics are non-additive (values can't be added up), so we can't use the `date_trunc` function to change their time grain granularity.
* By default, we take the first value of the period. You can change this by specifying a different function using the `period_agg` parameter.

In the following example, we define a cumulative metric, `cumulative_revenue`, that calculates the cumulative revenue for all orders:

models/marts/fct\_orders.yml

```yaml
metrics:
  - name: cumulative_revenue
    description: "The cumulative revenue for all orders."
    label: "Cumulative revenue (all-time)"
    type: cumulative
    input_metric: revenue
    period_agg: first # Optional. Defaults to first. Accepted values: first|last|average
```

In this example, `period_agg` is set to `first`, which chooses the first value for the selected granularity window. To query `cumulative_revenue` by week, use the following query syntax:

* `dbt sl query --metrics cumulative_revenue --group-by metric_time__week`

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

* When a period is specified, MetricFlow applies a sliding window to the underlying simple metric, such as tracking weekly active users with a 7-day window.
* Without specifying a period, cumulative metrics accumulate values over all time, useful for running totals like current revenue and active subscriptions.

 Example of window specified

If a window option is specified, MetricFlow applies a sliding window to the underlying metric.

Suppose the underlying metrics, `customers`, is configured to count the unique customers making orders at the Jaffle shop:

models/marts/customers.yml

```yaml
models:
  - name: customers
    semantic_model:
      enabled: true
      ...

    metrics:
      - name: customers
        description: "Unique customers making orders at the Jaffle shop"
        label: "Customers"
        type: simple
        agg: count_distinct
        expr: customer_id
```

We can write a cumulative metric `weekly_customers` as such:

models/marts/customers.yml

```yaml
metrics:
  - name: weekly_customers
    description: "Weekly active customers with a 7-day sliding window"
    label: "Weekly Active Customers"
    type: cumulative
    input_metric: customers
    window: 7 days
    period_agg: first # When using non-default granularity with cumulative metrics, re-aggregation is required. period_agg: first selects the first value in each granularity window during re-aggregation.
```

From the sample YAML example, note the following:

* `type`: Specify cumulative to indicate the type of metric.
* `input_metric`: Specify the metric to be aggregated (in this case, `customers`).
* `window`: Specify the accumulation window (in this case, 7 days).
* `period_agg`: Specify the re-aggregation function (in this case, `first`).
* `grain_to_date`: Specify the grain to date (in this case, `week`).

For example, in the `weekly_customers` cumulative metric, MetricFlow takes a sliding 7-day window of relevant customers and applies a count distinct function.

If you remove `window`, the metric will accumulate over all time.

 Example of window not specified

Suppose you (a subscription-based company for the sake of this example) have an event-based log table with the following columns:

* `date`: a date column
* `user_id`: (integer) an ID specified for each user that is responsible for the event
* `subscription_plan`: (integer) a column that indicates a particular subscription plan associated with the user.
* `subscription_revenue`: (integer) a column that indicates the value associated with the subscription plan.
* `event_type`: (integer) a column that populates with +1 to indicate an added subscription, or -1 to indicate a deleted subscription.
* `revenue`: (integer) a column that multiplies `event_type` and `subscription_revenue` to depict the amount of revenue added or lost for a specific date.

Using cumulative metrics without specifying a window, you can calculate running totals for metrics like the count of active subscriptions and revenue at any point in time. The following YAML file shows creating a cumulative metrics to obtain current revenue and the total number of active subscriptions as a cumulative sum using the `revenue` and `subscription_count` metrics:

models/marts/fct\_orders.yml

```yaml
metrics:
  - name: current_revenue
    description: "Current revenue"
    label: "Current Revenue"
    type: cumulative
    input_metric: revenue
  - name: active_subscriptions
    description: "Count of active subscriptions"
    label: "Active Subscriptions"
    type: cumulative
    input_metric: subscription_count
```

<!-- -->

### Grain to date[​](#grain-to-date "Direct link to Grain to date")

You can choose to specify a grain to date in your cumulative metric configuration to accumulate a metric from the start of a grain (such as week, month, or year). When using a window, such as a month, MetricFlow will go back one full calendar month. However, grain to date will always start accumulating from the beginning of the grain, regardless of the latest date of data.

<!-- -->

For example, let's consider an underlying simple metric `order_total` defined within a semantic model:

```yaml
models:
  - name: fct_orders
    semantic_model:
      enabled: true
    
    # Simple metrics defined within semantic model
    metrics:
      - name: order_total
        description: "Sum of order total amounts"
        type: simple
        agg: sum
        expr: order_total
```

We can compare the difference between a 1-month window and a monthly grain to date.

* The cumulative metric in a window approach applies a sliding window of 1 month
* The grain to date by month resets at the beginning of each month.

<!-- -->

models/marts/fct\_orders.yml

```yaml
# Top-level metrics key for cumulative metrics
metrics:
  - name: cumulative_order_total_mtd
    label: "Cumulative order total (MTD)"
    description: "The month-to-date value of all orders"
    type: cumulative
    grain_to_date: month
    input_metric: order_total
```

Cumulative metric with grain to date:

<!-- -->

models/marts/fct\_orders.yml

```yaml
# Top-level metrics key for cumulative metrics
metrics:
  - name: orders_last_month_to_date
    label: "Orders month to date"
    description: "Month-to-date count of orders"
    type: cumulative
    grain_to_date: month
    input_metric: order_count
```

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
