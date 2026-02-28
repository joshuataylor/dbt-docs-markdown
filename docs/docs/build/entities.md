# Entities

Entities are real-world concepts in a business, such as customers, transactions, and ad campaigns. We often focus our analyses on specific entities, such as customer churn or annual recurring revenue modeling. In our Semantic Layer models, these entities serve as a join key across semantic models.

<!-- -->

<!-- -->

Entities can be specified with a single column or multiple columns. Entities (join keys) in a semantic model are identified by their name. Each entity name must be unique within a semantic model, but it doesn't have to be unique across different semantic models.

There are four entity types:

* [Primary](#primary) — Has only one record for each row in the table and includes every record in the data platform. This key uniquely identifies each record in the table.
* [Unique](#unique) — Contains only one record per row in the table and allows for null values. May have a subset of records in the data warehouse.
* [Foreign](#foreign) — A field (or a set of fields) in one table that uniquely identifies a row in another table. This key establishes a link between tables.
* [Natural](#natural) — Columns or combinations of columns in a table that uniquely identify a record based on real-world data. This key is derived from actual data attributes.

Use entities as dimensions

You can also use entities as dimensions, which allows you to aggregate a metric to the granularity of that entity.

## Entity types[​](#entity-types "Direct link to Entity types")

MetricFlow's join logic depends on the entity `type` you use and determines how to join semantic models. Refer to [Joins](https://docs.getdbt.com/docs/build/join-logic.md) for more info on how to construct joins.

### Primary[​](#primary "Direct link to Primary")

A primary key has *only one* record for each row in the table and includes every record in the data platform. It must contain unique values and can't contain null values. Use the primary key to ensure that each record in the table is distinct and identifiable.

 Primary key example

For example, consider a table of employees with the following columns:

```sql
employee_id (primary key)
first_name
last_name
```

In this case, `employee_id` is the primary key. Each `employee_id` is unique and represents one specific employee. There can be no duplicate `employee_id` and can't be null.

### Unique[​](#unique "Direct link to Unique")

A unique key contains *only one* record per row in the table but may have a subset of records in the data warehouse. However, unlike the primary key, a unique key allows for null values. The unique key ensures that the column's values are distinct, except for null values.

 Unique key example

For example, consider a table of students with the following columns:

```sql
student_id (primary key)
email (unique key)
first_name
last_name
```

In this example, `email` is defined as a unique key. Each email address must be unique; however, multiple students can have null email addresses. This is because the unique key constraint allows for one or more null values, but non-null values must be unique. This then creates a set of records with unique emails (non-null) that could be a subset of the entire table, which includes all students.

### Foreign[​](#foreign "Direct link to Foreign")

A foreign key is a field (or a set of fields) in one table that uniquely identifies a row in another table. The foreign key establishes a link between the data in two tables. It can include zero, one, or multiple instances of the same record. It can also contain null values.

 Foreign key example

For example, consider you have two tables, `customers` and `orders`:

customers table:

```sql
customer_id (primary key)
customer_name
```

orders table:

```sql
order_id (primary key)
order_date
customer_id (foreign key)
```

In this example, the `customer_id` in the `orders` table is a foreign key that references the `customer_id` in the `customers` table. This link means each order is associated with a specific customer. However, not every order must have a customer; the `customer_id` in the orders table can be null or have the same `customer_id` for multiple orders.

### Natural[​](#natural "Direct link to Natural")

Natural keys are columns or combinations of columns in a table that uniquely identify a record based on real-world data. For instance, if you have a `sales_person_department` dimension table, the `sales_person_id` can serve as a natural key. You can only use natural keys for [SCD type II dimensions](https://docs.getdbt.com/docs/build/dimensions.md#scd-type-ii).

## Entities configuration[​](#entities-configuration "Direct link to Entities configuration")

The following is the complete spec for entities:

<!-- -->

<!-- -->

Here's an example of how to define entities in a semantic model:

<!-- -->

<!-- -->

## Combine columns with a key[​](#combine-columns-with-a-key "Direct link to Combine columns with a key")

If a table doesn't have any key (like a primary key), use *surrogate combination* to form a key that will help you identify a record by combining two columns. This applies to any [entity type](https://docs.getdbt.com/docs/build/entities.md#entity-types). For example, you can combine `date_key` and `brand_code` from the `raw_brand_target_weekly` table to form a *surrogate key*. The following example creates a surrogate key by joining `date_key` and `brand_code` using a pipe (`|`) as a separator.

<!-- -->

<!-- -->

## Examples[​](#examples "Direct link to Examples")

As mentioned, entities serve as our join keys, using the unique entity name. Therefore, we can join a single `unique` key to multiple `foreign` keys.

Consider a `date_categories` table with the following columns:

```sql
date_id (primary key)
date_day (unique key)
fiscal_year_name
```

And an `orders` table with the following columns:

```sql
order_id (primary key)
ordered_at
delivered_at
order_total
```

How might we define our Semantic Layer YAML so that we can query `order_total` by `ordered_at` `fiscal_year_name`, and `delivered_at` `fiscal_year_name`?

<!-- -->

<!-- -->

<!-- -->

<!-- -->

With this configuration, our semantic models can join on `ordered_at = date_day` via the `ordered_at_entity`, and on `delivered_at = date_day` via the `delivered_at_entity`. To validate our output, we can run:

* `dbt sl query --metrics order_total --group-by ordered_at_entity__fiscal_year_name` or
* `dbt sl query --metrics order_total --group-by delivered_at_entity__fiscal_year_name`

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
