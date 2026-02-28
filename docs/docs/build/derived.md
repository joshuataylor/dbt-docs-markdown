# Derived metrics

In MetricFlow, derived metrics are metrics created by defining an expression using other metrics. They enable you to perform calculations with existing metrics. This is helpful for combining metrics and doing math functions on aggregated columns, like creating a profit metric.

The parameters, description, and type for derived metrics are:

<!-- -->

<!-- -->

The following displays the complete specification for derived metrics, along with an example.

<!-- -->

<!-- -->

For advanced data modeling, you can use `fill_nulls_with` and `join_to_timespine` to [set null metric values to zero](https://docs.getdbt.com/docs/build/fill-nulls-advanced.md), ensuring numeric values for every data row.

## Derived metrics example[​](#derived-metrics-example "Direct link to Derived metrics example")

<!-- -->

<!-- -->

## Derived metric offset[​](#derived-metric-offset "Direct link to Derived metric offset")

To perform calculations using a metric's value from a previous time period, you can add an offset parameter to a derived metric. For example, if you want to calculate period-over-period growth or track user retention, you can use this metric offset.

**Note:** You must include the [`metric_time` dimension](https://docs.getdbt.com/docs/build/dimensions.md#time) when querying a derived metric with an offset window.

The following example displays how you can calculate monthly revenue growth using a 1-month offset window:

<!-- -->

<!-- -->

### Offset windows and granularity[​](#offset-windows-and-granularity "Direct link to Offset windows and granularity")

You can query any granularity and offset window combination. The following example queries a metric with a 7-day offset and a monthly grain:

<!-- -->

<!-- -->

When you run the query `dbt sl query --metrics d7_booking_change --group-by metric_time__month` for the metric, here's how it's calculated. For dbt Core, you can use the `mf query` prefix.

1. Retrieve the raw, unaggregated dataset with the specified
   <!-- -->
   and dimensions at the smallest level of detail, which is currently 'day'.

2. Then, perform an offset join on the daily dataset, followed by performing a date trunc and aggregation to the requested granularity. For example, to calculate `d7_booking_change` for July 2017:

   <!-- -->

   * First, sum up all the booking values for each day in July to calculate the bookings metric.
   * The following table displays the range of days that make up this monthly aggregation.

|       | Orders | Metric\_time             |
| ----- | ------ | ------------------------ |
|       | 330    | 2017-07-31               |
|       | 7030   | 2017-07-30 to 2017-07-02 |
|       | 78     | 2017-07-01               |
| Total | 7438   | 2017-07-01               |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

3. Calculate July's bookings with a 7-day offset. The following table displays the range of days that make up this monthly aggregation. Note that the month begins 7 days later (offset by 7 days) on 2017-07-24.

|       | Orders | Metric\_time             |
| ----- | ------ | ------------------------ |
|       | 329    | 2017-07-24               |
|       | 6840   | 2017-07-23 to 2017-06-30 |
|       | 83     | 2017-06-24               |
| Total | 7252   | 2017-07-01               |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

4. Lastly, calculate the derived metric and return the final result set:

```bash
bookings - bookings_7_days_ago would be compile as 7438 - 7252 = 186. 
```

| d7\_booking\_change | metric\_time\_\_month |
| ------------------- | --------------------- |
| 186                 | 2017-07-01            |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Related docs[​](#related-docs "Direct link to Related docs")

* [Fill null values for simple, derived, or ratio metrics](https://docs.getdbt.com/docs/build/fill-nulls-advanced.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
