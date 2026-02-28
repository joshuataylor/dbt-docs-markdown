# Fill null values for metrics

Understanding and implementing strategies to fill null values in metrics is key for accurate analytics. This guide explains `fill_nulls_with` and `join_to_timespine` to ensure data completeness, helping end users make more informed decisions and enhancing your dbt workflows.

### About null values[​](#about-null-values "Direct link to About null values")

You can use `fill_nulls_with` to replace null values in metrics with a value like zero (or your chosen integer). This ensures every data row shows a numeric value.

This guide explains how to ensure there are no null values in your metrics:

* Use `fill_nulls_with` for `simple`, `cumulative`, and `conversion` metrics
* Use `join_to_timespine` and `fill_nulls_with` together for derived and ratio metrics to avoid null values appearing.

### Fill null values for simple metrics[​](#fill-null-values-for-simple-metrics "Direct link to Fill null values for simple metrics")

For example, if you'd like to handle days with site visits but no leads, you can use `fill_nulls_with` to set the value for leads to zero on days when there are no conversions.

Let's say you have three metrics:

* `website_visits` and `leads`
* and a derived metric called `leads_to_website_visit` that calculates the ratio of leads to site visits.

<!-- -->

<!-- -->

The `website_visits` and `leads` metrics have the following data:

| metric\_time | website\_visits |
| ------------ | --------------- |
| 2024-01-01   | 50              |
| 2024-01-02   | 37              |
| 2024-01-03   | 79              |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

| metric\_time | leads |
| ------------ | ----- |
| 2024-01-01   | 5     |
| 2024-01-03   | 8     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

* Note that there is no data for `2024-01-02` in the `leads` metric.

Although there are no days without visits, there are days without leads. After applying `fill_nulls_with: 0` to the `leads` metric, querying these metrics together shows zero for leads on days with no conversions:

| metric\_time | website\_visits | leads |
| ------------ | --------------- | ----- |
| 2024-01-01   | 50              | 5     |
| 2024-01-02   | 37              | 0     |
| 2024-01-03   | 79              | 8     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Use join\_to\_timespine for derived and ratio metrics[​](#use-join_to_timespine-for-derived-and-ratio-metrics "Direct link to Use join_to_timespine for derived and ratio metrics")

<!-- -->

<!-- -->

### Fill null values for derived and ratio metrics[​](#fill-null-values-for-derived-and-ratio-metrics "Direct link to Fill null values for derived and ratio metrics")

To fill null values for derived and ratio metrics, you can link them with a time spine to ensure daily data coverage. As mentioned in [the previous section](#use-join_to_timespine-for-derived-and-ratio-metrics), this is because `derived` and `ratio` metrics take *metrics* as inputs<!-- -->.

For example, the following structure leaves nulls in the final results (`leads_to_website_visit` column) because `COALESCE` isn't applied at the third outer rendering layer for the final metric calculation in `derived` metrics:

| metric\_time | website\_visits | leads | leads\_to\_website\_visit |
| ------------ | --------------- | ----- | ------------------------- |
| 2024-01-01   | 50              | 5     | .1                        |
| 2024-01-02   | 37              | 0     | null                      |
| 2024-01-03   | 79              | 8     | .1                        |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

To display a zero value for `leads_to_website_visit` for `2024-01-02`, you would join the `leads` metric to a time spine model to ensure a value for each day. You can do this by adding `join_to_timespine` to the <!-- -->in the `leads` metric configuration:

<!-- -->

<!-- -->

Once you do this, if you query the `leads` metric after the timespine join, there will be a record for each day and any null values will get filled with zero.

| metric\_time | leads | leads\_to\_website\_visit |
| ------------ | ----- | ------------------------- |
| 2024-01-01   | 5     | .1                        |
| 2024-01-02   | 0     | 0                         |
| 2024-01-03   | 8     | .1                        |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Now, if you combine the metrics in a `derived` metric, there will be a zero value for `leads_to_website_visit` on `2024-01-02` and the final result set will not have any null values.

## FAQs[​](#faqs "Direct link to FAQs")

 How to handle null values in derived metrics defined on top of multiple tables

For additional examples and discussion on how to handle null values in derived metrics that use data from multiple tables, check out [MetricFlow issue #1031](https://github.com/dbt-labs/metricflow/issues/1031).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
