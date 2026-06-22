# SQL SUM

The SQL SUM function is handy and ever-present in data work. Let’s unpack what it is, how to use it, and why it's valuable.

Jumping into it, the SUM aggregate function allows you to calculate the sum of a numeric column or across a set of rows for a column. Ultimately, the SUM function is incredibly useful for calculating meaningful business metrics, such as Lifetime Value (LTV), and creating key numeric fields in [`fct_` and `dim_` models](https://www.getdbt.com/blog/guide-to-dimensional-modeling).

## How to use the SUM function in a query[​](#how-to-use-the-sum-function-in-a-query "Direct link to How to use the SUM function in a query")

Use the following syntax in a query to find the sum of a numeric field:

`sum(<field_name>)`

Since SUM is an aggregate function, you’ll need a GROUP BY statement in your query if you’re looking at counts broken out by dimension(s). If you’re calculating the standalone sum of a numeric field without the need to break them down by another field, you don’t need a GROUP BY statement.

SUM can also be used as a window function to operate across specified or partitioned rows. You can additionally pass a DISTINCT statement into a SUM function to only sum distinct values in a column.

Let’s take a look at a practical example using the SUM function below.

### SUM example[​](#sum-example "Direct link to SUM example")

The following example is querying from a sample dataset created by dbt Labs called [jaffle\_shop](https://github.com/dbt-labs/jaffle_shop):

```sql
select
	customer_id,
	sum(order_amount) as all_orders_amount
from {{ ref('orders') }}
group by 1
limit 3
```

This simple query is returning the summed amount of all orders for a customer in the Jaffle Shop’s `orders` table:

| customer\_id | all\_orders\_amount |
| ------------ | ------------------- |
| 1            | 33                  |
| 3            | 65                  |
| 94           | 24                  |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## SQL SUM function syntax in Snowflake, Databricks, BigQuery, and Redshift[​](#sql-sum-function-syntax-in-snowflake-databricks-bigquery-and-redshift "Direct link to SQL SUM function syntax in Snowflake, Databricks, BigQuery, and Redshift")

All modern data warehouses support the ability to use the SUM function (and follow the same syntax).

## SUM function use cases[​](#sum-function-use-cases "Direct link to SUM function use cases")

We most commonly see queries using SUM to:

* Calculate the cumulative sum of a metric across a customer/user id using a CASE WHEN statement (ex. `sum(case when order_array is not null then 1 else 0 end) as count_orders`)
* Create [dbt metrics](https://docs.getdbt.com/docs/build/build-metrics-intro.md) for key business values, such as LTV
* Calculate the total of a field across a dimension (ex. total session time, total time spent per ticket) that you typically use in `fct_` or `dim_` models
* Summing clicks, spend, impressions, and other key ad reporting metrics in tables from ad platforms

This isn’t an extensive list of where your team may be using SUM throughout your development work, dbt models, and BI tool logic, but it contains some common scenarios analytics engineers face day-to-day.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
