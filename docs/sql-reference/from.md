# SQL FROM

What makes the analytics world go ‘round? Queries and bad graphs. (Since we’re here to keep it brief, we won’t go into the latter here 😉)

The first thing someone learns in SQL: how to build a query using [SELECT](https://docs.getdbt.com/sql-reference/select.md) and FROM statements. The SQL FROM statement is the fundamental building block of any query: it allows you to identify the database schema object (table/view) you want to select data from in a query.

In a dbt project, a SQL dbt model is technically a singular SELECT statement (often built leveraging CTEs or subqueries) using a [reference](https://docs.getdbt.com/reference/dbt-jinja-functions/ref.md) to an upstream data model or table in a FROM statement.

## How to use SQL FROM statements[​](#how-to-use-sql-from-statements "Direct link to How to use SQL FROM statements")

Any query begins with a simple SELECT statement and wrapped up with a FROM statement:

```sql
select
	order_id, --select your columns
	customer_id,
	order_date
from {{ ref('orders') }} --the table/view/model you want to select from
limit 3
```

Woah woah woah! That is not the typical FROM statement you’re probably used to seeing!

Most FROM statements in the non-dbt world, such as when you’re running ad-hoc queries directly in your data warehouse, will follow the `FROM database.schema.table_name` syntax. In dbt projects, analytics engineers leverage [the ref statement](https://docs.getdbt.com/reference/dbt-jinja-functions/ref.md) to refer to other data models and sources to automatically build a dependency graph and avoid having to hard-code schema names. This flexibility is valuable as analytics engineers develop in their own development environments (schemas) without having to rename tables in their FROM statements.

This basic query is selecting three columns from the [Jaffle Shop’s](https://github.com/dbt-labs/jaffle_shop/blob/main/models/orders.sql) `orders` table and returning three rows. If you execute this query in your data warehouse, it will return a result looking like this:

| **order\_id** | **customer\_id** | **order\_date** |
| ------------- | ---------------- | --------------- |
| 1             | 1                | 2018-01-01      |
| 2             | 3                | 2018-01-02      |
| 3             | 95               | 2018-01-04      |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

In the query above, dbt automatically compiles the `from {{ ref('orders') }}` to `from analytics.jaffle_shop.orders` when the query is sent down to the data warehouse and run in the production environment.

If you’re selecting from multiple tables or models, that’s where you’d rely on unions or joins to bring multiple tables together in a way that makes sense to your data.

## FROM statement syntax in Snowflake, Databricks, BigQuery, and Redshift[​](#from-statement-syntax-in-snowflake-databricks-bigquery-and-redshift "Direct link to FROM statement syntax in Snowflake, Databricks, BigQuery, and Redshift")

Just as the humble SELECT statement is a SQL fundamental that goes untampered by the data warehouses, FROM syntax does not vary within them. As a result, writing the actual `select…from` statement across Snowflake, Databricks, Google BigQuery, and Amazon Redshift would look the same.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
