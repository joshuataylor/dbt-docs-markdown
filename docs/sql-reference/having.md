# SQL HAVING

SQL HAVING is just one of those little things that are going to make your ad hoc data work a little easier.

A not-so-fun fact about the [WHERE clause](https://docs.getdbt.com/sql-reference/where.md) is that you can’t filter on aggregates with it…that’s where HAVING comes in. With HAVING, you can not only define an aggregate in a [select](https://docs.getdbt.com/sql-reference/select.md) statement, but also filter on that newly created aggregate within the HAVING clause.

This page will walk through how to use HAVING, when you should use it, and discuss data warehouse support for it.

## How to use the HAVING clause in SQL[​](#how-to-use-the-having-clause-in-sql "Direct link to How to use the HAVING clause in SQL")

The HAVING clause essentially requires one thing: an aggregate field to evaluate. Since HAVING is technically a boolean, it will return rows that execute to true, similar to the WHERE clause.

The HAVING condition is followed after a [GROUP BY statement](https://docs.getdbt.com/sql-reference/group-by.md) and optionally enclosed with an ORDER BY statement:

```sql
select
	-- query
from <table>
group by <field(s)>
having condition
[optional order by]
```

That example syntax looks a little gibberish without some real fields, so let’s dive into a practical example using HAVING.

### SQL HAVING example[​](#sql-having-example "Direct link to SQL HAVING example")

* HAVING example
* CTE example

```sql
select
    customer_id,
    count(order_id) as num_orders
from {{ ref('orders') }}
group by 1
having num_orders > 1 --if you replace this with `where`, this query would not successfully run
```

```sql
with counts as (
	select
		customer_id,
		count(order_id) as num_orders
	from {{ ref('orders') }}
	group by 1
)
select
	customer_id,
	num_orders
from counts
where num_orders > 1
```

This simple query using the sample dataset [Jaffle Shop’s](https://github.com/dbt-labs/jaffle_shop) `orders` table will return customers who have had more than one order:

| customer\_id | num\_orders |
| ------------ | ----------- |
| 1            | 2           |
| 3            | 3           |
| 94           | 2           |
| 64           | 2           |
| 54           | 4           |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

The query above using the CTE utilizes more lines compared to the simpler query using HAVING, but will produce the same result.

## SQL HAVING clause syntax in Snowflake, Databricks, BigQuery, and Redshift[​](#sql-having-clause-syntax-in-snowflake-databricks-bigquery-and-redshift "Direct link to SQL HAVING clause syntax in Snowflake, Databricks, BigQuery, and Redshift")

[Snowflake](https://docs.snowflake.com/en/sql-reference/constructs/having.html), [Databricks](https://docs.databricks.com/sql/language-manual/sql-ref-syntax-qry-select-having.html), [BigQuery](https://cloud.google.com/bigquery/docs/reference/standard-sql/query-syntax#having_clause), and [Redshift](https://docs.aws.amazon.com/redshift/latest/dg/r_HAVING_clause.html) all support the HAVING clause and the syntax for using HAVING is the same across each of those data warehouses.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
