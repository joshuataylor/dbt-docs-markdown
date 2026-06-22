# SQL ARRAY\_AGG

In any typical programming language such as Python or Javascript, arrays are typically innate and bountiful; when you’re processing data in SQL, arrays are a little less common but are a handy way to provide more structure to your data.

To create an array of multiple data values in SQL, you’ll likely leverage the ARRAY\_AGG function (short for *array aggregation*), which puts your input column values into an array.

## How to use SQL ARRAY\_AGG[​](#how-to-use-sql-array_agg "Direct link to How to use SQL ARRAY_AGG")

The ARRAY\_AGG function has the following syntax:

`array_agg( [distinct] <field_name>) [within group (<order_by field>) over ([partition by <field>])`

A few notes on the functionality of this function:

* Most of the example syntax from above is optional, meaning the ARRAY\_AGG function can be as simple as `array_agg(<field_name>)` or used as a more complex as a window function
* [DISTINCT](https://docs.getdbt.com/sql-reference/distinct.md) is an optional argument that can be passed in, so only distinct values are in the return array
* If input column is empty, the returning array will also be empty
* Since the ARRAY\_AGG is an aggregate function (gasp!), you’ll need a GROUP BY statement at the end of your query if you’re grouping by certain field
* ARRAY\_AGG and similar aggregate functions can become inefficient or costly to compute on large datasets, so use ARRAY\_AGG wisely and truly understand your use cases for having arrays in your datasets

Let’s dive into a practical example using the ARRAY\_AGG function.

### SQL ARRAY\_AGG example[​](#sql-array_agg-example "Direct link to SQL ARRAY_AGG example")

```sql
select
    date_trunc('month', order_date) as order_month,
    array_agg(distinct status) as status_array
from  {{ ref('orders') }}
group by 1
order by 1
```

This simple query using the sample dataset [Jaffle Shop’s](https://github.com/dbt-labs/jaffle_shop) `orders` table is returning a new column of distinct order statuses by order month:

| order\_month | status\_array                                   |
| ------------ | ----------------------------------------------- |
| 2018-01-01   | \[ "returned", "completed", "return\_pending" ] |
| 2018-02-01   | \[ "completed", "return\_pending" ]             |
| 2018-03-01   | \[ "completed", "shipped", "placed" ]           |
| 2018-04-01   | \[ "placed" ]                                   |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Looking at the query results—this makes sense! We’d expect newer orders to likely not have any returns, and older orders to have completed returns.

## SQL ARRAY\_AGG syntax in Snowflake, Databricks, BigQuery, and Redshift[​](#sql-array_agg-syntax-in-snowflake-databricks-bigquery-and-redshift "Direct link to SQL ARRAY_AGG syntax in Snowflake, Databricks, BigQuery, and Redshift")

[Snowflake](https://docs.snowflake.com/en/sql-reference/functions/array_agg.html), [Databricks](https://docs.databricks.com/sql/language-manual/functions/array_agg.html), and [BigQuery](https://cloud.google.com/bigquery/docs/reference/standard-sql/aggregate_functions#array_agg) all support the ARRAY\_AGG function. Redshift, however, supports an out-of-the-box [LISTAGG function](https://docs.aws.amazon.com/redshift/latest/dg/r_LISTAGG.html) that can perform similar functionality to ARRAY\_AGG. The primary difference is that LISTAGG allows you to explicitly choose a delimiter to separate a list whereas arrays are naturally delimited by commas.

## ARRAY\_AGG use cases[​](#array_agg-use-cases "Direct link to ARRAY_AGG use cases")

There are definitely too many use cases to list out for using the ARRAY\_AGG function in your dbt models, but it’s very likely that ARRAY\_AGG is used pretty downstream in your DAG since you likely don’t want your data so bundled up earlier in your DAG to improve modularity and dryness. A few downstream use cases for ARRAY\_AGG:

* In [`export_` models](https://www.getdbt.com/open-source-data-culture/reverse-etl-playbook) that are used to send data to platforms using a reverse ETL tool to pair down multiple rows into a single row. Some downstream platforms, for example, require certain values that we’d usually keep as separate rows to be one singular row per customer or user. ARRAY\_AGG is handy to bring multiple column values together by a singular id, such as creating an array of all items a user has ever purchased and sending that array downstream to an email platform to create a custom email campaign.
* Similar to export models, you may see ARRAY\_AGG used in [mart tables](https://docs.getdbt.com/best-practices/how-we-structure/4-marts.md) to create final aggregate arrays per a singular dimension; performance concerns of ARRAY\_AGG in these likely larger tables can potentially be bypassed with use of [incremental models in dbt](https://docs.getdbt.com/docs/build/incremental-models.md).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
