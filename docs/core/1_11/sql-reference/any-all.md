# SQL ANY and ALL

The SQL ANY and ALL operators are useful for evaluating conditions to limit query results; they are often passed in with [LIKE](https://docs.getdbt.com/sql-reference/like.md) and [ILIKE](https://docs.getdbt.com/sql-reference/ilike.md) operators. The ANY operator will return true if any of the conditions passed into evaluate to true, while ALL will only return true if *all* conditions passed into it are true.

Use this page to better understand how to use ANY and ALL operators, use cases for these operators, and which data warehouses support them.

## How to use the SQL ANY and ALL operators[​](#how-to-use-the-sql-any-and-all-operators "Direct link to How to use the SQL ANY and ALL operators")

The ANY and ALL operators have very simple syntax and are often passed in the LIKE/ILIKE operator or subquery:

`where <field_name> like/ilike any/all (array_of_options)`

`where <field_name> = any/all (subquery)`

Some notes on this operator’s syntax and functionality:

* You may pass in a subquery into the ANY or ALL operator instead of an array of options
* Use the ILIKE operator with ANY or ALL to avoid case sensitivity

Let’s dive into a practical example using the ANY operator now.

### SQL ANY example[​](#sql-any-example "Direct link to SQL ANY example")

```sql
select
    order_id,
    status
from {{ ref('orders') }}
where status like any ('return%', 'ship%')
```

This simple query using the [Jaffle Shop’s](https://github.com/dbt-labs/jaffle_shop) `orders` table will return orders whose status is like the patterns `start with 'return'` or `start with 'ship'`:

| order\_id | status          |
| --------- | --------------- |
| 18        | returned        |
| 23        | return\_pending |
| 74        | shipped         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Because LIKE is case-sensitive, it would not return results in this query for orders whose status were say `RETURNED` or `SHIPPED`. If you have a mix of uppercase and lowercase strings in your data, consider standardizing casing for strings using the [UPPER](https://docs.getdbt.com/sql-reference/upper.md) and [LOWER](https://docs.getdbt.com/sql-reference/lower.md) functions or use the more flexible ILIKE operator.

## ANY and ALL syntax in Snowflake, Databricks, BigQuery, and Redshift[​](#any-and-all-syntax-in-snowflake-databricks-bigquery-and-redshift "Direct link to ANY and ALL syntax in Snowflake, Databricks, BigQuery, and Redshift")

Snowflake and Databricks support the ability to use ANY in a LIKE operator. Amazon Redshift and Google BigQuery, however, do not support the use of ANY in a LIKE or ILIKE operator. Use the table below to read more on the documentation for the ANY operator in your data warehouse.

| **Data warehouse**                                                                | **ANY support?**                                                                                                                                                            | **ALL support?**                                                                                         |
| --------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| [Snowflake](https://docs.snowflake.com/en/sql-reference/functions/like_any.html)  | ✅                                                                                                                                                                          | ✅                                                                                                       |
| [Databricks](https://docs.databricks.com/sql/language-manual/functions/like.html) | ✅                                                                                                                                                                          | ✅                                                                                                       |
| Amazon Redshift                                                                   | ❌Not supported; consider utilizing multiple OR clauses or [IN operators](https://docs.getdbt.com/sql-reference/in.md).                                                     | ❌Not supported; consider utilizing multiple [AND clauses](https://docs.getdbt.com/sql-reference/and.md) |
| Google BigQuery                                                                   | ❌Not supported; consider utilizing [multiple OR clauses](https://stackoverflow.com/questions/54645666/how-to-implement-like-any-in-bigquery-standard-sql) or IN operators. | ❌Not supported; consider utilizing multiple AND clauses                                                 |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
