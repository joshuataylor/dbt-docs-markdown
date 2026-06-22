# SQL OUTER JOIN

SQL full outer joins exist and therefore we have to talk about them, but they’re *highly unlikely* to be a join you regularly leverage in your data work. In plain terms, a SQL full outer join is a join between two tables that returns *all rows* from both tables, regardless of join key match success; compare this to [left](https://docs.getdbt.com/sql-reference/left-join.md), [inner](https://docs.getdbt.com/sql-reference/outer-join.md), or [right joins](https://docs.getdbt.com/sql-reference/right-join.md) that require matches to be successful to return certain rows.

In this page, we’ll unpack how to create a full outer join and demonstrate when you might need one in your analytics engineering work.

## How to create a full outer join[​](#how-to-create-a-full-outer-join "Direct link to How to create a full outer join")

Like all joins, you need some database objects (ie tables/views), keys to join on, and a [select statement](https://docs.getdbt.com/sql-reference/select.md) to perform a full outer join:

```text
select
    <fields>
from <table_1> as t1
full outer join <table_1> as t2
on t1.id = t2.id 
```

In this example above, there’s only one field being used to join the table together; if you’re joining between database objects that require multiple fields, you can leverage AND/OR operators, and more preferably, surrogate keys. You may additionally add [WHERE](https://docs.getdbt.com/sql-reference/where.md), [GROUP BY](https://docs.getdbt.com/sql-reference/group-by.md), [ORDER BY](https://docs.getdbt.com/sql-reference/order-by.md), [HAVING](https://docs.getdbt.com/sql-reference/having.md), and other clauses after your joins to create filtering, ordering, and performing aggregations.

A note on full outer joins: it may sound obvious, but because full outer joins can return all rows between two tables, they therefore can return *many* rows, which is not necessarily a recipe for efficiency. When you use full outer joins, you often can find alternatives using different joins or unions to potentially bypass major inefficiencies caused by a full outer join.

### SQL full outer join example[​](#sql-full-outer-join-example "Direct link to SQL full outer join example")

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
full outer join {{ ref('car_color') }} as car_color
on car_type.user_id = car_color.user_id
order by 1
```

This simple query will return all rows from tables A and B, regardless of `user_id` match success between the two tables:

| user\_id | type  | color  |
| -------- | ----- | ------ |
| 1        | van   | red    |
| 2        | sedan | null   |
| 3        | truck | green  |
| 4        | null  | yellow |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## SQL full outer join use cases[​](#sql-full-outer-join-use-cases "Direct link to SQL full outer join use cases")

There will inevitably be valid use cases for full outer joins in your dbt project. However, because of the nature of dbt, which heavily encourages modularity and DRY dryness, the necessity for full outer joins may go down (slightly). Regardless, the two primary cases for full outer joins we typically see are around consolidating or merging multiple entities together and data validation.

* Merging tables together: A full outer join between two tables can bring those entities together, regardless of join key match. This type of joining can often be bypassed by using different joins, unions, pivots, and a combination of these, but hey, sometimes the full outer join is a little less work 🤷
* Data validation: Full outer joins can be incredibly useful when performing data validation; for example, in the [dbt-audit-helper package](https://github.com/dbt-labs/dbt-audit-helper), a full outer join is used in the [compare\_column\_values test](https://github.com/dbt-labs/dbt-audit-helper/blob/main/macros/compare_column_values.sql) to help determine where column values are mismatched between two dbt models.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
