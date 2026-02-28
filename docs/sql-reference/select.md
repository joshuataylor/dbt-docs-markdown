# SQL SELECT

My goodness, would there even be modern data teams without SQL SELECT statements? Probably not.

Luckily, we live in a world of tabular data, cloud data warehouses, and SQL prowess. Analysts and analytics engineers are writing queries, creating data models, and leveraging SQL to power their [data transformations](https://www.getdbt.com/analytics-engineering/transformation/) and analysis. But what makes these queries possible? SELECT statements.

The SQL SELECT statement is the fundamental building block of any query: it allows you to select specific columns (data) from a database schema object (table/view). In a dbt project, a SQL dbt model is technically a singular SELECT statement (often built leveraging CTEs or subqueries).

## How to use SELECT[​](#how-to-use-select "Direct link to How to use SELECT")

Any query begins with a simple SELECT statement:

```sql
select
	order_id, --your first column you want selected
	customer_id, --your second column you want selected
	order_date --your last column you want selected (and so on)
from {{ ref('orders') }} --the table/view/model you want to select from
limit 3
```

This basic query is selecting three columns from the [jaffle shop’s](https://github.com/dbt-labs/jaffle_shop/blob/main/models/orders.sql) `orders` table and returning three rows. If you execute this query in your data warehouse, it will return a result looking like this:

| order\_id | customer\_id | order\_date |
| --------- | ------------ | ----------- |
| 1         | 1            | 2018-01-01  |
| 2         | 3            | 2018-01-02  |
| 3         | 95           | 2018-01-04  |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

You may also commonly see queries that `select * from table_name`. The asterisk or star is telling the compiler to select all columns from a specified table or view.

Goodbye carpal tunnel

Leverage [dbt utils’ star macro](https://docs.getdbt.com/blog/star-sql-love-letter) to be able to both easily select many and specifically exclude certain columns.

In a dbt project, analytics engineers will typically write models that contain multiple CTEs that build to one greater query. For folks that are newer to analytics engineering or dbt, we recommend they check out the [“How we structure our dbt projects” guide](https://docs.getdbt.com/best-practices/how-we-structure/1-guide-overview.md) to better understand why analytics folks like modular data modeling and CTEs.

## SELECT statement syntax in Snowflake, Databricks, BigQuery, and Redshift[​](#select-statement-syntax-in-snowflake-databricks-bigquery-and-redshift "Direct link to SELECT statement syntax in Snowflake, Databricks, BigQuery, and Redshift")

While we know the data warehouse players like to have their own slightly different flavors and syntax for SQL, they have conferred together that the SELECT statement is sacred and unchangeable. As a result, writing the actual `select…from` statement across Snowflake, Databricks, Google BigQuery, and Amazon Redshift would look the same. However, the actual SQL manipulation of data within the SELECT statement (ex. adding dates, casting columns) might look slightly different between each data warehouse.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
