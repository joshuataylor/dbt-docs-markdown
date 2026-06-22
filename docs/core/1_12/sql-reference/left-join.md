# SQL LEFT JOIN

An analytics engineer favorite: the left join. Without a doubt, this is probably the most regularly used join in any dbt project (and for good reason).

The left join returns all rows in the [FROM statement](https://docs.getdbt.com/sql-reference/from.md), regardless of match in the left join database object. Compare this to an [inner join](https://docs.getdbt.com/sql-reference/inner-join.md), where only rows are returned that have successful key matches between the database object in the FROM statement and in the inner join statement.

## How to create a left join[​](#how-to-create-a-left-join "Direct link to How to create a left join")

Like all joins, you need some database objects (ie tables/views), keys to join on, and a [select statement](https://docs.getdbt.com/sql-reference/select.md) to perform a left join:

```text
select
    <fields>
from <table_1> as t1
left join <table_2> as t2
on t1.id = t2.id 
```

In this example above, there’s only one field from each table being used to join the two together together; if you’re joining between two database objects that require multiple fields, you can leverage AND/OR operators, and more preferably, surrogate keys. You may additionally add [WHERE](https://docs.getdbt.com/sql-reference/where.md), [GROUP BY](https://docs.getdbt.com/sql-reference/group-by.md), [ORDER BY](https://docs.getdbt.com/sql-reference/order-by.md), [HAVING](https://docs.getdbt.com/sql-reference/having.md), and other clauses after your joins to create filtering, ordering, and performing aggregations. You may also left (or any join really) as many joins as you’d like in an individual query or CTE.

### SQL left join example[​](#sql-left-join-example "Direct link to SQL left join example")

Table A `car_type`

| **user\_id** | **car\_type** |
| ------------ | ------------- |
| 1            | van           |
| 2            | sedan         |
| 3            | truck         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Table B `car_color`

| user\_id | car\_color |
| -------- | ---------- |
| 1        | red        |
| 3        | green      |
| 4        | yellow     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

```sql
select
   car_type.user_id as user_id,
   car_type.car_type as type,
   car_color.car_color as color
from {{ ref('car_type') }} as car_type
left join {{ ref('car_color') }} as car_color
on car_type.user_id = car_color.user_id
```

This simple query will return *all rows* from Table A and adds the `color` column to rows where there’s a successful match to Table B:

| **user\_id** | **type** | **color** |
| ------------ | -------- | --------- |
| 1            | van      | red       |
| 2            | sedan    | null      |
| 3            | truck    | green     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Because there’s no `user_id` = 2 in Table B, there is no `color` available, thus a null result `color` column for `user_id` 2.

## SQL left join use cases[​](#sql-left-join-use-cases "Direct link to SQL left join use cases")

Left joins are a fundamental in data modeling and analytics engineering work—they allow you to easily join database objects onto each other while maintaining an original table’s row count (in the from statement). Compared to right joins, that return all rows in a right join database object (and not the from statement), we find left joins a little more intuitive to understand and build off of.

Ensure your joins are just ~~left~~ right

Something to note if you use left joins: if there are multiple records for an individual key in the left join database object, be aware that duplicates can potentially be introduced in the final query result. This is where dbt tests, such as testing for primary key uniqueness and [equal row count](https://github.com/dbt-labs/dbt-utils#equal_rowcount-source) across upstream source tables and downstream child models, can help you identify faulty data modeling logic and improve data quality.

Where you will not (and should not) see left joins is in [staging models](https://docs.getdbt.com/best-practices/how-we-structure/2-staging.md) that are used to clean and prep raw source data for analytics uses. Any joins in your dbt projects should happen further downstream in [intermediate](https://docs.getdbt.com/best-practices/how-we-structure/3-intermediate.md) and [mart models](https://docs.getdbt.com/best-practices/how-we-structure/4-marts.md) to improve modularity and DAG cleanliness.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
