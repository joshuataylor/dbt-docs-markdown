# SQL SELF JOINS

Simultaneously the easiest and most confusing of joins: the self join. Simply put, a self join allows you to join a dataset back onto itself.

If you’re newer to data work or SQL, you may be asking yourself: why in the world would you want to do this? Shouldn’t joins happen between multiple *different* entities?

The majority of joins you see in analytics work and dbt projects will probably be left and inner joins, but occasionally, depending on how the raw source table is built out, you’ll leverage a self join. One of the most common use cases to leverage a self join is when a table contains a foreign key to the primary key of that same table.

It’s ok if none of that made sense—jump into this page to better understand how and where you might use a self join in your analytics engineering work.

## How to create a self join[​](#how-to-create-a-self-join "Direct link to How to create a self join")

No funny venn diagrams here—there’s actually even no special syntax for self joins. To create a self join, you’ll use a regular join syntax, the only differences is the join objects are *the same*:

```text
select
	<fields>
from <table_1> as t1
[<join_type>] join <table_2> as t2
on t1.id = t2.id
```

Since you can choose the dialect of join for a self join, you can specify if you want to do a [left](https://docs.getdbt.com/sql-reference/left-join.md), [outer](https://docs.getdbt.com/sql-reference/outer-join.md), [inner](https://docs.getdbt.com/sql-reference/inner-join.md), [cross](https://docs.getdbt.com/sql-reference/cross-join.md), or [right join](https://docs.getdbt.com/sql-reference/right-join.md) in the join statement.

### SQL self join example[​](#sql-self-join-example "Direct link to SQL self join example")

Given a `products` table that looks likes this, where there exists both a primary key (`sku_id`) and foreign key (`parent_id`) to that primary key:

| **sku\_id** | **sku\_name** | **parent\_id** |
| ----------- | ------------- | -------------- |
| 1           | Lilieth Bed   | 4              |
| 2           | Holloway Desk | 3              |
| 3           | Basic Desk    | null           |
| 4           | Basic Bed     | null           |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

And this query utilizing a self join to join `parent_name` onto skus:

```sql
select
   products.sku_id,
   products.sku_name,
   products.parent_id,
   parents.sku_name as parent_name
from {{ ref('products') }} as products
left join {{ ref('products') }} as parents
on products.parent_id = parents.sku_id
```

This query utilizing a self join adds the `parent_name` of skus that have non-null `parent_ids`:

| sku\_id | sku\_name     | parent\_id | parent\_name |
| ------- | ------------- | ---------- | ------------ |
| 1       | Lilieth Bed   | 4          | Basic Bed    |
| 2       | Holloway Desk | 3          | Basic Desk   |
| 3       | Basic Desk    | null       | null         |
| 4       | Basic Bed     | null       | null         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## SQL self join use cases[​](#sql-self-join-use-cases "Direct link to SQL self join use cases")

Again, self joins are probably rare in your dbt project and will most often be utilized in tables that contain a hierarchical structure, such as consisting of a column which is a foreign key to the primary key of the same table. If you do have use cases for self joins, such as in the example above, you’ll typically want to perform that self join early upstream in your DAG, such as in a [staging](https://docs.getdbt.com/best-practices/how-we-structure/2-staging.md) or [intermediate](https://docs.getdbt.com/best-practices/how-we-structure/3-intermediate.md) model; if your raw, unjoined table is going to need to be accessed further downstream sans self join, that self join should happen in a modular intermediate model.

You can also use self joins to create a cartesian product (aka a cross join) of a table against itself. Again, slim use cases, but still there for you if you need it 😉

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
