# SQL ROW\_NUMBER

In this page, let’s go deep into the ROW\_NUMBER function and talk about what it is, how to use it, and why it’s important in analytics engineering work.

The ROW\_NUMBER window function is an effective way to create a ranked column or filter a query based on rankings. More specifically, the ROW\_NUMBER function returns the *unique* row number of a row in an ordered group or dataset.

Unlike the [RANK](https://docs.getdbt.com/sql-reference/rank.md) and DENSE\_RANK functions, ROW\_NUMBER is non-deterministic, meaning that a *unique* number is assigned arbitrarily for rows with duplicate values.

## How to use the ROW\_NUMBER function[​](#how-to-use-the-row_number-function "Direct link to How to use the ROW_NUMBER function")

The ROW\_NUMBER function has a pretty simple syntax, with an optional partition field and support for ordering customization:

`row_number() over ([partition by <field(s)>] order by field(s) [asc | desc])`

Some notes on this function’s syntax:

* The `partition by` field is optional; if you want to get the row numbers of your entire dataset (compared to grabbing row number within a group of rows in your dataset), you would simply omit the `partition by` from the function call (see the example below for this).
* By default, the ordering of a ROW\_NUMBER function is set to ascending. To explicitly make the resulting order descending, you’ll need to pass in `desc` to the `order by` part of the function.

Let’s take a look at a practical example using the ROW\_NUMBER function below.

### ROW\_NUMBER function example[​](#row_number-function-example "Direct link to ROW_NUMBER function example")

```sql
select
    customer_id,
    order_id,
    order_date,
    row_number() over (partition by customer_id order by order_date) as row_n
from {{ ref('orders') }}
order by 1
```

This simple query using the [Jaffle Shop’s](https://github.com/dbt-labs/jaffle_shop) `orders` table will return the unique row number per customer by their `order_date`:

| customer\_id | order\_id | order\_date | row\_n |
| ------------ | --------- | ----------- | ------ |
| 1            | 1         | 2018-01-01  | 1      |
| 1            | 37        | 2018-02-10  | 2      |
| 2            | 8         | 2018-01-11  | 1      |
| 3            | 2         | 2018-01-02  | 1      |
| 3            | 24        | 2018-01-27  | 2      |
| 3            | 69        | 2018-03-11  | 3      |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Because ROW\_NUMBER is non-deterministic, orders per customer that have the same `order_date` would have unique `row_n` values (unlike if you used the RANK or DENSE\_RANK functions).

## ROW\_NUMBER syntax in Snowflake, Databricks, BigQuery, and Redshift[​](#row_number-syntax-in-snowflake-databricks-bigquery-and-redshift "Direct link to ROW_NUMBER syntax in Snowflake, Databricks, BigQuery, and Redshift")

Most, if not all, modern data warehouses support ROW\_NUMBER and other similar ranking functions; the syntax is also the same across them. Use the table below to read more on the documentation for the ROW\_NUMBER function in your data warehouse.

| Data warehouse                                                                                                  | ROW\_NUMBER support? |
| --------------------------------------------------------------------------------------------------------------- | -------------------- |
| [Snowflake](https://docs.snowflake.com/en/sql-reference/functions/row_number.html)                              | ✅                   |
| [Databricks](https://docs.databricks.com/sql/language-manual/functions/row_number.html)                         | ✅                   |
| [Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/dg/r_WF_ROW_NUMBER.html)                          | ✅                   |
| [Google BigQuery](https://cloud.google.com/bigquery/docs/reference/standard-sql/numbering_functions#row_number) | ✅                   |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## ROW\_NUMBER function use cases[​](#row_number-function-use-cases "Direct link to ROW_NUMBER function use cases")

We most commonly see the ROW\_NUMBER function used in data work to:

* In [SELECT statements](https://docs.getdbt.com/sql-reference/select.md) to add explicit and unique row numbers in a group of data or across an entire table
* Paired with QUALIFY statement, filter CTEs, queries, or models to capture one unique row per specified partition with the ROW\_NUMBER function. This is particularly useful when you need to remove duplicate rows from a dataset (but use this wisely!).

This isn’t an extensive list of where your team may be using the ROW\_NUMBER function throughout your dbts some common scenarios analytics engineers face day-to-day.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
