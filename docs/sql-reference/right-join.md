# SQL RIGHT JOIN

The not-as-favorite child: the right join. Unlike [left joins](https://docs.getdbt.com/sql-reference/left-join.md) that return all rows in the database object in [the FROM statement](https://docs.getdbt.com/sql-reference/from.md), regardless of match in the left join object, right joins return all rows *in the right join database object*, regardless of match in the database object in the FROM statement.

What you really need to know: You can accomplish anything a right join does with a left join and left joins typically are more readable and intuitive. However, we’ll still walk you through how to use right joins and elaborate on why we think left joins are superior 😉

## How to create a right join[​](#how-to-create-a-right-join "Direct link to How to create a right join")

Like all joins, you need some database objects (ie tables/views), keys to join on, and a [select statement](https://docs.getdbt.com/sql-reference/select.md) to perform a right join:

```text
select
    <fields>
from <table_1> as t1
right join <table_2> as t2
on t1.id = t2.id 
```

In this example above, there’s only one field from each table being used to join the two together together; if you’re joining between two database objects that require multiple fields, you can leverage AND/OR operators, and more preferably, surrogate keys. You may additionally add [WHERE](https://docs.getdbt.com/sql-reference/where.md), [GROUP BY](https://docs.getdbt.com/sql-reference/group-by.md), [ORDER BY](https://docs.getdbt.com/sql-reference/order-by.md), [HAVING](https://docs.getdbt.com/sql-reference/having.md), and other clauses after your joins to create filtering, ordering, and performing aggregations. You may also right (or any join really) as many joins as you’d like in an individual query or CTE.

### SQL right join example[​](#sql-right-join-example "Direct link to SQL right join example")

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

| **user\_id** | **car\_color** |
| ------------ | -------------- |
| 1            | red            |
| 3            | green          |
| 4            | yellow         |

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
right join {{ ref('car_color') }} as car_color
on car_type.user_id = car_color.user_id
```

This simple query will return *all* rows from Table B and adds the `color` column to rows where there’s a successful match to Table A:

| **user\_id** | **type** | **color** |
| ------------ | -------- | --------- |
| 1            | van      | red       |
| 3            | truck    | green     |
| 4            | null     | yellow    |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Because there’s no `user_id` = 4 in Table A, there is no `type` available, thus a null result `type` column for `user_id` 4. Since no `user_id` = 2 exists in Table B, and that id is not in the right join database object, no rows with a `user_id` of 2 will be returned.

## SQL right join use cases[​](#sql-right-join-use-cases "Direct link to SQL right join use cases")

Compared to left joins, you likely won’t see right joins as often (or ever) in data modeling and analytics engineering work. But why not?

Simply because right joins are a little less intuitive than a left join. When you’re data modeling, you’re usually focused on one database object, and adding the supplementary data or tables you need to give you a final dataset. That one focal database object is typically what is put in the `from {{ ref('my_database_object')}}`; any other columns that are joined onto it from other tables are usually supplementary, but keeping all the rows from the initial table of focus is usually the priority. Don’t get us wrong—right joins can get you there—it’s likely just a little less intuitive and can get complex with queries that involve multiple joins.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
