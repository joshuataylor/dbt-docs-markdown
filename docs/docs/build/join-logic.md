# Joins

Joins are a powerful part of MetricFlow and simplify the process of making all valid dimensions available for your metrics at query time, regardless of where they are defined in different semantic models. With Joins, you can also create metrics using <!-- -->from different semantic models.

Joins use `entities` defined in your semantic model configs as the join keys between tables. Assuming entities are defined in the semantic model, MetricFlow creates a graph using the semantic models as nodes and the join paths as edges to perform joins automatically. MetricFlow chooses the appropriate join type and avoids fan-out or chasm joins with other tables based on the entity types.

 What are fan-out or chasm joins?

* Fan-out joins are when one row in a table is joined to multiple rows in another table, resulting in more output rows than input rows.
* Chasm joins are when two tables have a many-to-many relationship through an intermediate table, and the join results in duplicate or missing data.

## Types of joins[​](#types-of-joins "Direct link to Types of joins")

Joins are auto-generated

MetricFlow automatically generates the necessary joins to the defined semantic objects, eliminating the need for you to create new semantic models or configuration files.

This section explains the different types of joins that can be used with entities and how to query them.

Metricflow uses these specific join strategies:

* Primarily uses left joins when joining `fct` and `dim` models. Left joins make sure all rows from the "base" table are retained, while matching rows are included from the joined table.
* For queries that involve multiple `fct` models, MetricFlow uses full outer joins to ensure all data points are captured, even when some `dim` or `fct` models are missing in certain tables.
* MetricFlow restricts the use of fan-out and chasm joins.

Refer to [SQL examples](#sql-examples) for more information on how MetricFlow handles joins in practice.

The following table identifies which joins are allowed based on specific entity types to prevent the creation of risky joins. This table primarily represents left joins unless otherwise specified. For scenarios involving multiple `fct` models, MetricFlow uses full outer joins.

| entity type - Table A | entity type - Table B | Join type                |
| --------------------- | --------------------- | ------------------------ |
| Primary               | Primary               | ✅ Left                  |
| Primary               | Unique                | ✅ Left                  |
| Primary               | Foreign               | ❌ Fan-out (Not allowed) |
| Unique                | Primary               | ✅ Left                  |
| Unique                | Unique                | ✅ Left                  |
| Unique                | Foreign               | ❌ Fan-out (Not allowed) |
| Foreign               | Primary               | ✅ Left                  |
| Foreign               | Unique                | ✅ Left                  |
| Foreign               | Foreign               | ❌ Fan-out (Not allowed) |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Semantic validation[​](#semantic-validation "Direct link to Semantic validation")

MetricFlow performs semantic validation by executing `explain` queries in the data platform to ensure that the generated SQL gets executed without errors. This validation includes:

* Verifying that all referenced tables and columns exist.
* Ensuring the data platform supports SQL functions, such as `date_diff(x, y)`.
* Checking for ambiguous joins or paths in multi-hop joins.

If validation fails, MetricFlow surfaces errors for users to address before executing the query.

## Example[​](#example "Direct link to Example")

The following example uses two semantic models with a common entity and shows a MetricFlow query that requires a join between the two semantic models: `transactions` and `user_signup`.

<!-- -->

<!-- -->

```yaml
dbt sl query --metrics average_purchase_price --group-by metric_time,user_id__type # In dbt platform />
```

```yaml
mf query --metrics average_purchase_price --group-by metric_time,user_id__type # In dbt Core
```

#### SQL examples[​](#sql-examples "Direct link to SQL examples")

These SQL examples show how MetricFlow handles both left join and full outer join scenarios in practice:

* SQL example for left join
* SQL example for outer joins

Using the previous example for `transactions` and `user_signup` semantic models, this shows a left join between those two semantic models.

```sql
select
  transactions.user_id,
  transactions.purchase_price,
  user_signup.type
from transactions
left outer join user_signup
  on transactions.user_id = user_signup.user_id
where transactions.purchase_price is not null
group by
  transactions.user_id,
  user_signup.type;
```

If you have multiple `fct` models, let's say `sales` and `returns`, MetricFlow uses full outer joins to ensure all data points are captured.

This example shows a full outer join between the `sales` and `returns` semantic models.

```sql
select
  sales.user_id,
  sales.total_sales,
  returns.total_returns
from sales
full outer join returns
  on sales.user_id = returns.user_id
where sales.user_id is not null or returns.user_id is not null;
```

## Multi-hop joins[​](#multi-hop-joins "Direct link to Multi-hop joins")

MetricFlow allows users to join <!-- -->and dimensions across a graph of entities by moving from one table to another within a graph. This is referred to as "multi-hop join".

MetricFlow can join up to three tables, supporting multi-hop joins with a limit of two hops. This does the following:

* Enables complex data analysis without ambiguous paths.
* Supports navigating through data models, like moving from `orders` to `customers` to `country` tables.

While direct three-hop paths are limited to prevent confusion from multiple routes to the same data, MetricFlow does allow joining more than three tables if the joins don’t exceed two hops to reach a dimension.

For example, if you have two models, `country` and `region`, where customers are linked to countries, which in turn are linked to regions, you can join all of them in a single SQL query and can dissect `orders` by `customer__country_country_name` but not by `customer__country__region_name`.

![Multi-Hop-Join](/assets/images/multihop-diagram-03171b81496cb0fd452d2c2f0b5e0ed3.png "Example schema for reference")

<!-- -->

<!-- -->

### Query multi-hop joins[​](#query-multi-hop-joins "Direct link to Query multi-hop joins")

To query dimensions *without* a multi-hop join involved, you can use the fully qualified dimension name with the syntax entity double underscore (dunder) dimension, like `entity__dimension`.

For dimensions retrieved by a multi-hop join, you need to additionally provide the entity path as a list, like `user_id`.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
