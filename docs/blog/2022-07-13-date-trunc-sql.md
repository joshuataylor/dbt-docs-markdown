# DATE\_TRUNC SQL function: Why we love it

July 13, 2022 · 5 min read

[![Kira Furuichi](/img/blog/authors/kira-furuichi.png)](./authors/kira-furuichi.md)

[Kira Furuichi](./authors/kira-furuichi.md)

Technical Writer at dbt Labs

In general, data people prefer the more granular over the less granular. [Timestamps > dates](https://docs.getdbt.com/blog/when-backend-devs-spark-joy#signs-the-data-is-sparking-joy), daily data > weekly data, etc.; having data at a more granular level always allows you to zoom in. However, you’re likely looking at your data at a somewhat zoomed-out level—weekly, monthly, or even yearly. To do that, you’re going to need a handy dandy function that helps you round out date or time fields.

The DATE\_TRUNC function will truncate a date or time to the first instance of a given date part. Wordy, wordy, wordy! What does this really mean? If you were to truncate `2021-12-13` out to its month, it would return `2021-12-01` (the first day of the month).

Using the DATE\_TRUNC function, you can truncate to the weeks, months, years, or other date parts for a date or time field. This can make date/time fields easier to read, as well as help perform cleaner time-based analyses.

> **What is a SQL function?** At a high level, a function takes an input (or multiple inputs) and returns a manipulation of those inputs. Some common SQL functions are [COALESCE](https://getdbt.com/sql-foundations/coalesce-sql-love-letter/), [LOWER](https://getdbt.com/sql-foundations/lower-sql-love-letter/), and [EXTRACT](https://getdbt.com/sql-foundations/extract-sql-love-letter/). For example, the COALESCE function takes a group of values and returns the first non-null value from that group.

Overall, it’s a great function to use to help you aggregate your data into specific date parts while keeping a date format. However, the DATE\_TRUNC function isn’t your swiss army knife–it’s not able to do magic or solve all of your problems (we’re looking at you [star](https://getdbt.com/sql-foundations/star-sql-love-letter/)). Instead, DATE\_TRUNC is your standard kitchen knife—it’s simple and efficient, and you almost never start cooking (data modeling) without it.

## How to use the DATE\_TRUNC function

For the DATE\_TRUNC function, there are two arguments you must pass in:

* The date part: This is the days/months/weeks/years (level) you want your field to be truncated out to
* The date/time you want to be truncated

The DATE\_TRUNC function can be used in SELECT statements and WHERE clauses.

Most, if not all, modern cloud data warehouses support some type of the DATE\_TRUNC function. There may be some minor differences between the argument order for DATE\_TRUNC across data warehouses, but the functionality very much remains the same.

Below, we’ll outline some of the slight differences in the implementation between some of the data warehouses.

### The DATE\_TRUNC function in Snowflake and Databricks

In [Snowflake](https://docs.snowflake.com/en/sql-reference/functions/date_trunc.html) and [Databricks](https://docs.databricks.com/sql/language-manual/functions/date_trunc.html), you can use the DATE\_TRUNC function using the following syntax:

```sql
date_trunc(<date_part>, <date/time field>)
```

In these platforms, the `<date_part>` is passed in as the first argument in the DATE\_TRUNC function.

### The DATE\_TRUNC function in Google BigQuery and Amazon Redshift

In [Google BigQuery](https://cloud.google.com/bigquery/docs/reference/standard-sql/date_functions#date_trunc) and [Amazon Redshift](https://docs.aws.amazon.com/redshift/latest/dg/r_DATE_TRUNC.html), the `<date_part>` is passed in as the first argument and the `<date/time field>` is the second argument.

```sql
date_trunc(<date/time field>, <date part>)
```

> **A note on BigQuery:** BigQuery’s DATE\_TRUNC function supports the truncation of date types, whereas Snowflake, Redshift, and Databricks’ `date/time field` can be a date or timestamp data type. BigQuery also supports DATETIME\_TRUNC and TIMESTAMP\_TRUNC functions to support truncation of more granular date/time types.

## A dbt macro to remember

Why Snowflake, Amazon Redshift, Databricks, and Google BigQuery decided to use different implementations of essentially the same function is beyond us and it’s not worth the headache trying to figure that out. Instead of remembering if the`<date_part>` or the `<date/time field>` comes first, (which, let’s be honest, we can literally never remember) you can rely on a dbt macro to help you get away from finicky syntax.

With dbt v1.2, [adapters](https://docs.getdbt.com/docs/supported-data-platforms) now support [cross-database macros](https://docs.getdbt.com/reference/dbt-jinja-functions/cross-database-macros) to help you write certain functions, like [DATE\_TRUNC](https://docs.getdbt.com/reference/dbt-jinja-functions/cross-database-macros#date_trunc) and [DATEDIFF](https://docs.getdbt.com/reference/dbt-jinja-functions/cross-database-macros#datediff), without having to memorize sticky function syntax.

> **Note:** Previously, [dbt\_utils](https://github.com/dbt-labs/dbt-utils), a package of macros and tests that data folks can use to help write more DRY code in their dbt project, powered cross-database macros. Now, cross-database macros are available **regardless if dbt utils is installed or not.**

Using the [jaffle shop](https://github.com/dbt-labs/jaffle_shop/blob/main/models/orders.sql), a simple dataset and dbt project, you can truncate the `order_date` from the `orders` table using the dbt [DATE\_TRUNC macro](https://docs.getdbt.com/reference/dbt-jinja-functions/cross-database-macros#date_trunc):

```sql
select
	order_id,
	order_date,
	{{ date_trunc("week", "order_date") }} as order_week,
	{{ date_trunc("month", "order_date") }} as order_month,
	{{ date_trunc("year", "order_date") }} as order_year
from {{ ref('orders') }}
```

Running the above would product the following sample results:

| order\_id | order\_date | order\_week | order\_month | order\_year |
| --------- | ----------- | ----------- | ------------ | ----------- |
| 1         | 2018-01-01  | 2018-01-01  | 2018-01-01   | 2018-01-01  |
| 70        | 2018-03-12  | 2018-03-12  | 2018-03-01   | 2018-01-01  |
| 91        | 2018-03-31  | 2018-03-26  | 2018-03-01   | 2018-01-01  |

The `order_week`, `order_month`, and `order_year` fields are the truncated values from the `order_date` field.

**A mild word of warning:** If you’re using the DATE\_TRUNC function to modify fields or create new ones, it’s important that you use strong naming conventions for these fields. Since the output from the DATE\_TRUNC function looks like a normal date, other data folks or business users may not understand that it’s an altered field and may mistake it for the actual date something happened.

## Why we love it

The DATE\_TRUNC function is a great way to do data analysis and data modeling that needs to happen at a zoomed-out date part. It’s often used for time-based work, such as customer retention modeling or analysis. The DATE\_TRUNC function also allows you to keep the date format of a field which allows for the most ease and compatibility in most BI (business intelligence) tools.

TL;DR – DATE\_TRUNC is a handy, widely-used SQL function—and dbt has made it even simpler to start using!

*This post is a part of the SQL love letters—a series on the SQL functions the dbt Labs data team members use and love. You can find [the entire collection here](https://getdbt.com/sql-foundations/top-sql-functions).*

**Tags:**

* [sql magic](./tags/sql-magic.md)

[Newer post](./2022-07-14-change-data-capture.md)

[Strategies for change data capture in dbt](./2022-07-14-change-data-capture.md)

[Older post](./2022-07-13-datediff-sql-love-letter.md)

[DATEDIFF SQL function: Why we love it](./2022-07-13-datediff-sql-love-letter.md)
