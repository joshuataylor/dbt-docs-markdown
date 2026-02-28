# SQL CONCAT

There is no better or simpler way to join multiple string values in a query than by using the CONCAT function. Full stop.

It’s a straightforward function with pretty straightforward use cases. Use this page to understand how to use the CONCAT function in your data warehouse and why analytics engineers use it throughout their dbt models.

## How to use the CONCAT function[​](#how-to-use-the-concat-function "Direct link to How to use the CONCAT function")

Using the CONCAT function is pretty straightforward: you’ll pass in the strings or binary values you want to join together in the correct order into the CONCAT function. You can pass in as many expressions into the CONCAT function as you would like.

### CONCAT function example[​](#concat-function-example "Direct link to CONCAT function example")

```sql
select
	user_id,
	first_name,
	last_name,
	concat(first_name, ' ', last_name) as full_name
from {{ ref('customers') }}
limit 3
```

This query using the [Jaffle Shop’s](https://github.com/dbt-labs/jaffle_shop) `customers` table will return results like this with a new column of the combined `first_name` and `last_name` field with a space between them:

| user\_id | first\_name | last\_name | full\_name  |
| -------- | ----------- | ---------- | ----------- |
| 1        | Michael     | P.         | Michael P.  |
| 2        | Shawn       | M.         | Shawn M.    |
| 3        | Kathleen    | P.         | Kathleen P. |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## CONCAT function syntax in Snowflake, Databricks, BigQuery, and Redshift[​](#concat-function-syntax-in-snowflake-databricks-bigquery-and-redshift "Direct link to CONCAT function syntax in Snowflake, Databricks, BigQuery, and Redshift")

Snowflake, Databricks, Google BigQuery, and Amazon Redshift all support the CONCAT function with the syntax looking the same in each platform. You may additionally see the CONCAT function represented by the `||` operator (ex. `select first_name || last_name AS full_name from {{ ref('customers') }}`) which has the same functionality as the CONCAT function in these data platforms.

## CONCAT use cases[​](#concat-use-cases "Direct link to CONCAT use cases")

We most commonly see concatenation in SQL for strings to:

* Join together address/geo columns into one field
* Add hard-coded string values to columns to create clearer column values
* Create surrogate keys using a hashing method and multiple column values (ex. `md5(column_1 || column_2) as unique_id`

This isn’t an extensive list of where your team may be using CONCAT throughout your data work, but it contains some common scenarios analytics engineers face day-to-day.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
