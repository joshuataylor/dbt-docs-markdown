# SQL CROSS JOIN

A truly rarely seen, but important join: the cross join. The majority of your analytics engineering work will require you to join tables together to create robust, wide tables that will eventually be exposed to end business users. These models will usually be created using mostly [left](https://docs.getdbt.com/sql-reference/left-join.md) (and some [inner](https://docs.getdbt.com/sql-reference/inner-join.md)) joins.

A cross join, on the other hand, typically takes two columns between two database objects and creates a table forming a combination of all rows across joined tables, called a cartesian product. Use this page to understand how to use cross joins and where you might leverage them in your dbt project.

## How to create a cross join[​](#how-to-create-a-cross-join "Direct link to How to create a cross join")

Unlike regular joins, cross joins don’t use keys to join database objects together:

```text
select
    <fields>
from <table_1> as t1
cross join <table_2> as t2
```

Cross joins are one of those SQL concepts that is easier to understand with a tangible example, so let’s jump into it.

### SQL cross join example[​](#sql-cross-join-example "Direct link to SQL cross join example")

Table A `date_spine`

| date       |
| ---------- |
| 2022-01-01 |
| 2022-01-02 |
| 2022-01-03 |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Table B `users`

| user\_id |
| -------- |
| 1        |
| 3        |
| 4        |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

```sql
select
   users.user_id as user_id,
   date.date as date
from {{ ref('users') }} as users
cross join {{ ref('date_spine') }} as date
order by 1
```

This simple query will return a cartesian cross of all users and dates, essentially creating a unique combination of user per date per row:

| user\_id | type       |
| -------- | ---------- |
| 1        | 2022-01-01 |
| 1        | 2022-01-02 |
| 1        | 2022-01-03 |
| 2        | 2022-01-01 |
| 2        | 2022-01-02 |
| 2        | 2022-01-03 |
| 3        | 2022-01-01 |
| 3        | 2022-01-02 |
| 3        | 2022-01-03 |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Generate surrogate keys from cross joins

In the generated table above, the unique key is a combination of the `user_id` and `date` per row. To add a primary key to this table, you could generate a surrogate key using an MD5 hash the `generate_surrogate_key` macro in dbt-utils (ex. `{{ dbt_utils.generate_surrogate_key(user_id, type) }}` that could eventually be joined onto other tables.

## SQL cross join use case[​](#sql-cross-join-use-case "Direct link to SQL cross join use case")

When would the generated table above be useful? Cross joining unique dates and users can be an effective way to create a base table to join various event counts, such as key website, email, or product events, to. These report-type tables are useful to expose to end business users in BI tools to look at aggregate counts per day per user and other useful measures.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
