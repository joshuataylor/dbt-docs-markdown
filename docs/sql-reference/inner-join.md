# SQL INNER JOINS

The cleanest and easiest of SQL joins: the humble inner join. Just as its name suggests, an inner join between two database objects returns all rows that have matching join keys; any keys that don’t match are omitted from the query result.

## How to create an inner join[​](#how-to-create-an-inner-join "Direct link to How to create an inner join")

Like all joins, you need some database objects (ie tables/views), keys to join on, and a [select statement](https://docs.getdbt.com/sql-reference/select.md) to perform an inner join:

```text
select
    <fields>
from <table_1> as t1
inner join <table_2> as t2
on t1.id = t2.id 
```

In this example above, there’s only one field from each table being used to join the two together; if you’re joining between two database objects that require multiple fields, you can leverage AND/OR operators, and more preferably, surrogate keys. You may additionally add [WHERE](https://docs.getdbt.com/sql-reference/where.md), [GROUP BY](https://docs.getdbt.com/sql-reference/group-by.md), [ORDER BY](https://docs.getdbt.com/sql-reference/order-by.md), [HAVING](https://docs.getdbt.com/sql-reference/having.md), and other clauses after your joins to create filtering, ordering, and performing aggregations.

As with any query, you can perform as many joins as you want in a singular query. A general word of advice: try to keep data models modular by performing regular DAG audits. If you join certain tables further upstream, are those individual tables needed again further downstream? If your query involves multiple joins and complex logic and is exposed to end business users, ensure that you leverage table or [incremental materializations](https://docs.getdbt.com/docs/build/incremental-models.md).

### SQL inner join example[​](#sql-inner-join-example "Direct link to SQL inner join example")

Table A `car_type`

| user\_id | car\_type |
| -------- | --------- |
| 1        | van       |
| 2        | sedan     |
| 3        | truck     |

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
inner join {{ ref('car_color') }} as car_color
on car_type.user_id = car_color.user_id
```

This simple query will return all rows that have the same `user_id` in both Table A and Table B:

| user\_id | type  | color |
| -------- | ----- | ----- |
| 1        | van   | red   |
| 3        | truck | green |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Because there’s no `user_id` = 4 in Table A and no `user_id` = 2 in Table B, rows with ids 2 and 4 (from either table) are omitted from the inner join query results.

## SQL inner join use cases[​](#sql-inner-join-use-cases "Direct link to SQL inner join use cases")

There are probably countless scenarios where you’d want to inner join multiple tables together—perhaps you have some really nicely structured tables with the exact same primary keys that should really just be one larger, wider table or you’re joining two tables together don’t want any null or missing column values if you used a left or right join—it’s all pretty dependent on your source data and end use cases. Where you will not (and should not) see inner joins is in [staging models](https://docs.getdbt.com/best-practices/how-we-structure/2-staging.md) that are used to clean and prep raw source data for analytics uses. Any joins in your dbt projects should happen further downstream in [intermediate](https://docs.getdbt.com/best-practices/how-we-structure/3-intermediate.md) and [mart models](https://docs.getdbt.com/best-practices/how-we-structure/4-marts.md) to improve modularity and DAG cleanliness.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
