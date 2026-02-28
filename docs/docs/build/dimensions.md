# Dimensions

<!-- -->

All dimensions require a `name`, `type`, and can optionally include an `expr` parameter. The `name` for your Dimension must be unique within the same semantic model.

<!-- -->

<!-- -->

Refer to the following example to see how dimensions are used in a semantic model:

<!-- -->

<!-- -->

<!-- -->

<!-- -->

## Dimensions types[​](#dimensions-types "Direct link to Dimensions types")

This section further explains the dimension definitions, along with examples. Dimensions have the following types:

* [`derived_semantics` in `dimensions`](#derived_semantics-in-dimensions)

* [Dimensions types](#dimensions-types)

* [Categorical](#categorical)

* [Time](#time)

  * [SCD Type II](#scd-type-ii)

    * [Basic structure](#basic-structure)
    * [Semantic model parameters and keys](#semantic-model-parameters-and-keys)
    * [Implementation](#implementation)
    * [SCD examples](#scd-examples)

## Categorical[​](#categorical "Direct link to Categorical")

Categorical dimensions are used to group metrics by different attributes, features, or characteristics such as product type. They can refer to existing columns in your dbt model or be calculated using a SQL expression with the `expr` parameter. An example of a categorical dimension is `is_bulk_transaction`, which is a group created by applying a case statement to the underlying column `quantity`. This allows users to group or filter the data based on bulk transactions.

<!-- -->

## Time[​](#time "Direct link to Time")

<!-- -->

<!-- -->

```bash
# dbt users
dbt sl query --metrics users_created,users_deleted --group-by metric_time__year --order-by metric_time__year

# dbt Core users
mf query --metrics users_created,users_deleted --group-by metric_time__year --order-by metric_time__year
```

You can set `is_partition` for time to define specific time spans.

* is\_partition
* time\_granularity

Use `is_partition: True` to show that a dimension exists over a specific time window. For example, a date-partitioned dimensional table. When you query metrics from different tables, the Semantic Layer uses this parameter to ensure that the correct dimensional values are joined to measures.

### SCD Type II[​](#scd-type-ii "Direct link to SCD Type II")

<!-- -->

<!-- -->

MetricFlow supports joins against dimensions values in a semantic model built on top of a slowly changing dimension (SCD) Type II table. This is useful when you need a particular metric sliced by a group that changes over time, such as the historical trends of sales by a customer's country.

#### Basic structure[​](#basic-structure "Direct link to Basic structure")

SCD Type II are groups that change values at a coarser time granularity. SCD Type II tables typically have two time columns that indicate the validity period of a dimension: `valid_from` (or `tier_start`) and `valid_to` (or `tier_end`). This creates a range of valid rows with different dimension values for a metric<!-- -->.

MetricFlow associates the metric with the earliest available dimension value within a coarser time window, such as a month. By default, it uses the group valid at the start of this time granularity.

MetricFlow supports the following basic structure of an SCD Type II data platform table:

| entity\_key | dimensions\_1 | dimensions\_2 | ... | dimensions\_x | valid\_from | valid\_to  |
| ----------- | ------------- | ------------- | --- | ------------- | ----------- | ---------- |
| 123         | value\_a      | value\_x      | ... | value\_n      | 2024-01-01  | 2024-06-30 |
| 123         | value\_b      | value\_y      | ... | value\_m      | 2024-07-01  | 2024-12-31 |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

* `entity_key` (required): A unique identifier for each row in the table, such as a primary key or another unique identifier specific to the entity.
* `valid_from` (required): Start date timestamp for when the dimension is valid. Use `validity_params: is_start: True` in the semantic model to specify this.
* `valid_to` (required): End date timestamp for when the dimension is valid. Use `validity_params: is_end: True` in the semantic model to specify this.

#### Semantic model parameters and keys[​](#semantic-model-parameters-and-keys "Direct link to Semantic model parameters and keys")

When configuring an SCD Type II table in a semantic model, use `validity_params` to specify the start (`valid_from`) and end (`valid_to`) of the validity window for each dimension.

* `validity_params`: Parameters that define the validity window.

  <!-- -->

  * `is_start: True`: Indicates the start of the validity period. Displayed as `valid_from` in the SCD table.
  * `is_end: True`: Indicates the end of the validity period. Displayed as `valid_to` in the SCD table.

Here’s an example configuration:

<!-- -->

<!-- -->

SCD Type II tables have a specific dimension with a start and end date. To join tables:

* Set the additional [entity `type`](https://docs.getdbt.com/docs/build/entities.md#entity-types) parameter to the `natural` key.
* Use a `natural` key as an [entity `type`](https://docs.getdbt.com/docs/build/entities.md#entity-types), which means you don't need a `primary` key.
* In most instances, SCD tables don't have a logically usable `primary` key because `natural` keys map to multiple rows.

#### Implementation[​](#implementation "Direct link to Implementation")

Here are some guidelines to follow when implementing SCD Type II tables:

* The SCD table must have `valid_to` and `valid_from` time dimensions, which are logical constructs.
* The `valid_from` and `valid_to` properties must be specified exactly once per SCD table configuration.
* The `valid_from` and `valid_to` properties shouldn't be used or specified on the same time dimension.
* The `valid_from` and `valid_to` time dimensions must cover a non-overlapping period where one row matches each natural key value (meaning they must not overlap and should be distinct).
* We recommend defining the underlying dbt model with [dbt snapshots](https://docs.getdbt.com/docs/build/snapshots.md). This supports the SCD Type II table layout and ensures that the table is updated with the latest data.

This is an example of SQL code that shows how a sample metric called `num_events` is joined with versioned dimensions data (stored in a table called `scd_dimensions`) using a primary key made up of the `entity_key` and `timestamp` columns.

```sql
select metric_time, dimensions_1, sum(1) as num_events
from events a
left outer join scd_dimensions b
on 
  a.entity_key = b.entity_key 
  and a.metric_time >= b.valid_from 
  and (a.metric_time < b. valid_to or b.valid_to is null)
group by 1, 2
```

#### SCD examples[​](#scd-examples "Direct link to SCD examples")

The following are examples of how to use SCD Type II tables in a semantic model:

 SCD dimensions for sales tiers and the time length of that tier.

This example shows how to create slowly changing dimensions (SCD) using a semantic model. The SCD table contains information about salespersons' tier and the time length of that tier. Suppose you have the underlying SCD table:

| sales\_person\_id | tier | start\_date | end\_date  |
| ----------------- | ---- | ----------- | ---------- |
| 111               | 1    | 2019-02-03  | 2020-01-05 |
| 111               | 2    | 2020-01-05  | 2048-01-01 |
| 222               | 2    | 2020-03-05  | 2048-01-01 |
| 333               | 2    | 2020-08-19  | 2021-10-22 |
| 333               | 3    | 2021-10-22  | 2048-01-01 |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

As mentioned earlier, the `validity_params` include two important arguments that specify the columns in the SCD table that mark the start and end dates (or timestamps) for each tier or dimension:

* `is_start`
* `is_end`

Additionally, the entity is tagged as `natural` to differentiate it from a `primary` entity. In a `primary` entity, each entity value has one row. In contrast, a `natural` entity has one row for each combination of entity value and its validity period.

The following code represents a separate semantic model that holds a fact table for `transactions`:

You can now access the metrics in the `transactions` semantic model organized by the slowly changing dimension of `tier`.

In the sales tier example, For instance, if a salesperson was Tier 1 from 2022-03-01 to 2022-03-12, and gets promoted to Tier 2 from 2022-03-12 onwards, all transactions from March would be categorized under Tier 1 since the dimensions value of Tier 1 comes earlier (and is the default starting point), even though the salesperson was promoted to Tier 2 on 2022-03-12.

 SCD dimensions with sales tiers and group transactions by month when tiers are missing

This example shows how to create slowly changing dimensions (SCD) using a semantic model. The SCD table contains information about salespersons' tier and the time length of that tier. Suppose you have the underlying SCD table:

| sales\_person\_id | tier | start\_date | end\_date  |
| ----------------- | ---- | ----------- | ---------- |
| 111               | 1    | 2019-02-03  | 2020-01-05 |
| 111               | 2    | 2020-01-05  | 2048-01-01 |
| 222               | 2    | 2020-03-05  | 2048-01-01 |
| 333               | 2    | 2020-08-19  | 2021-10-22 |
| 333               | 3    | 2021-10-22  | 2048-01-01 |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

In the sales tier example, if sales\_person\_id 456 is Tier 2 from 2022-03-08 onwards, but there is no associated tier level dimension for this person from 2022-03-01 to 2022-03-08, then all transactions associated with sales\_person\_id 456 for the month of March will be grouped under 'NA' since no tier is present prior to Tier 2.

The following command or code represents how to return the count of transactions generated by each sales tier per month:

```bash
# dbt platform users
dbt sl query --metrics transactions --group-by metric_time__month,sales_person__tier --order-by metric_time__month,sales_person__tier

# dbt Core users
mf query --metrics transactions --group-by metric_time__month,sales_person__tier --order-by metric_time__month,sales_person__tier
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
