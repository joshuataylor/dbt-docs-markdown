# About MetricFlow

This guide introduces MetricFlow's fundamental ideas for people new to this feature. MetricFlow, which powers the Semantic Layer, helps you define and manage the logic for your company's metrics. It's an opinionated set of abstractions and helps data consumers retrieve metric datasets from a data platform quickly and efficiently.

MetricFlow handles SQL query construction and defines the specification for dbt semantic models and metrics. It allows you to define metrics in your dbt project and query them with [MetricFlow commands](https://docs.getdbt.com/docs/build/metricflow-commands.md) whether in dbt or dbt Core.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

Before you start, consider the following guidelines:

<!-- -->

<!-- -->

## MetricFlow[​](#metricflow "Direct link to MetricFlow")

MetricFlow is a SQL query generation tool designed to streamline metric creation across different data dimensions for diverse business needs.

* It operates through YAML files, where a semantic graph links language to data. This graph comprises [semantic models](https://docs.getdbt.com/docs/build/semantic-models.md) (data entry points) and [metrics](https://docs.getdbt.com/docs/build/metrics-overview.md) (functions for creating quantitative indicators).
* MetricFlow is developed and maintained as part of the [Open Semantic Interchange (OSI)](https://www.snowflake.com/en/blog/open-semantic-interchange-ai-standard/) initiative.
* MetricFlow is compatible with dbt version 1.6 and higher.
* MetricFlow is distributed under the [Apache 2.0 license](https://github.com/dbt-labs/metricflow/blob/main/LICENSE). Data practitioners and enthusiasts are highly encouraged to contribute. Read more about [MetricFlow's license history](https://github.com/dbt-labs/metricflow?tab=readme-ov-file#license-history).
* As a part of the Semantic Layer, MetricFlow empowers organizations to define metrics using YAML abstractions.
* To query metric dimensions, dimension values, and validate configurations, use [MetricFlow commands](https://docs.getdbt.com/docs/build/metricflow-commands.md).

note

MetricFlow doesn't support dbt [builtin functions or packages](https://docs.getdbt.com/reference/dbt-jinja-functions/builtins.md) at this time, however, support is planned for the future.

MetricFlow abides by these principles:

* **Flexibility with completeness**: Define metric logic using flexible abstractions on any data model.
* **DRY (Don't Repeat Yourself)**: Minimize redundancy by enabling metric definitions whenever possible.
* **Simplicity with gradual complexity:** Approach MetricFlow using familiar data modeling concepts.
* **Performance and efficiency**: Optimize performance while supporting centralized data engineering and distributed logic ownership.

### Semantic graph[​](#semantic-graph "Direct link to Semantic graph")

We're introducing a new concept: a "semantic graph". It's the relationship between semantic models and YAML configurations that creates a data landscape for building metrics. You can think of it like a map, where tables are like locations, and the connections between them (edges) are like roads. Although it's under the hood, the semantic graph is a subset of the DAG, and you can see the semantic models as nodes on the DAG.

The semantic graph helps us decide which information is available to use for consumption and which is not. The connections between tables in the semantic graph are more about relationships between the information. This is different from the DAG, where the connections show dependencies between tasks.

When MetricFlow generates a metric, it uses its SQL engine to figure out the best path between tables using the framework defined in YAML files for semantic models and metrics. When these models and metrics are correctly defined, they can be used downstream with Semantic Layer's integrations.

### Semantic models[​](#semantic-models "Direct link to Semantic models")

Semantic models are the starting points of your data and correspond to models in your dbt project. You can create multiple semantic models from each model. Semantic models have metadata, like a data table, that define important information such as the table name and primary keys for the graph to be navigated correctly.

For a semantic model, there are three main pieces of metadata:

* [Entities](https://docs.getdbt.com/docs/build/entities.md): The join keys of your semantic model (think of these as the traversal paths, or edges between semantic models).
* [Dimensions](https://docs.getdbt.com/docs/build/dimensions.md): These are the ways you want to group or slice/dice your metrics.

<!-- -->

<!-- -->

### Metrics[​](#metrics "Direct link to Metrics")

<!-- -->

<!-- -->

MetricFlow supports different metric types:

<!-- -->

<!-- -->

## Use case[​](#use-case "Direct link to Use case")

In the upcoming sections, we'll show how data practitioners currently calculate metrics and compare it to how MetricFlow makes defining metrics easier and more flexible.

The following example data is based on the Jaffle Shop repo. You can view the complete [dbt project](https://github.com/dbt-labs/jaffle-sl-template). The tables we're using in our example model are:

* `orders` is a production data platform export that has been cleaned up and organized for analytical consumption
* `customers` is a partially denormalized table in this case with a column derived from the orders table through some upstream process

To make this more concrete, consider the metric `order_total`, which is defined using the SQL expression:

`select sum(order_total) as order_total from orders` This expression calculates the total revenue for all orders by summing the `order_total` column in the orders table. In a business setting, the metric `order_total` is often calculated according to different categories, such as:

* Time, for example `date_trunc(ordered_at, 'day')`
* Order Type, using `is_food_order` dimension from the `orders` table

### Calculate metrics[​](#calculate-metrics "Direct link to Calculate metrics")

Next, we'll compare how data practitioners currently calculate metrics with multiple queries versus how MetricFlow simplifies and streamlines the process.

* Calculate with multiple queries
* Calculate with MetricFlow

The following example displays how data practitioners typically would calculate the `order_total` metric aggregated. It's also likely that analysts are asked for more details on a metric, like how much revenue came from new customers.

Using the following query creates a situation where multiple analysts working on the same data, each using their own query method — this can lead to confusion, inconsistencies, and a headache for data management.

```sql
select
    date_trunc('day',orders.ordered_at) as day, 
    case when customers.first_ordered_at is not null then true else false end as is_new_customer,
    sum(orders.order_total) as order_total
from
  orders
left join
  customers
on
  orders.customer_id = customers.customer_id
group by 1, 2
```

In the following three example tabs, use MetricFlow to define a semantic model that uses `order_total` as a metric and a sample schema to create consistent and accurate results — eliminating confusion, code duplication, and streamlining your workflow.

* Revenue example
* More dimensions example
* Advanced example

Similarly, you can add additional dimensions like `is_food_order` to your semantic models to incorporate even more dimensions to slice and dice your revenue `order_total`.

Imagine an even more complex metric is needed, such as the amount of money earned each day from food orders from returning customers. Without MetricFlow, the data practitioner's original SQL might look like this:

```sql
select
    date_trunc('day',orders.ordered_at) as day, 
    sum(case when is_food_order = true then order_total else null end) as food_order,
    sum(orders.order_total) as sum_order_total,
    food_order/sum_order_total
from
  orders
left join
  customers
on
  orders.customer_id = customers.customer_id
where
  case when customers.first_ordered_at is not null then true else false end = true
group by 1
```

MetricFlow simplifies the SQL process through metric YAML configurations as shown below. You can also commit them to your git repository to ensure everyone on the data and business teams can see and approve them as the true and only source of information.

## FAQs[​](#faqs "Direct link to FAQs")

 Do my datasets need to be normalized?

Not at all! While a cleaned and well-modeled dataset can be extraordinarily powerful and is the ideal input, you can use any dataset from raw to fully denormalized datasets.

It's recommended that you apply quality data consistency, such as filtering bad data, normalizing common objects, and data modeling of keys and tables, in upstream applications. The Semantic Layer is more efficient at doing data denormalization instead of normalization.

If you have not invested in data consistency, that is okay. The Semantic Layer can take SQL queries or expressions to define consistent datasets.

 Why is normalized data the ideal input?

MetricFlow is built to do denormalization efficiently. There are better tools to take raw datasets and accomplish the various tasks required to build data consistency and organized data models. On the other end, by putting in denormalized data you are potentially creating redundancy which is technically challenging to manage, and you are reducing the potential granularity that MetricFlow can use to aggregate metrics.

<!-- -->

 How does the dbt Semantic Layer handle joins?

The dbt Semantic Layer, powered by MetricFlow, builds joins based on the types of keys and parameters that are passed to entities. To better understand how joins are constructed, see the documentation on [join types](https://docs.getdbt.com/docs/build/join-logic.md#types-of-joins).

Rather than capturing arbitrary join logic, MetricFlow captures the types of each identifier and then helps users navigate to appropriate joins. This allows us to avoid the construction of fan out and chasm joins as well as generate legible SQL.

 Are entities and join keys the same thing?

If it helps you to think of entities as join keys, that is very reasonable. Entities in MetricFlow have applications beyond joining two tables, such as acting as a dimension.

 Can a table without a primary or unique entities have dimensions?

Yes, but because a dimension is considered an attribute of the primary or unique entity of the table, they are only usable by the metrics that are defined in that table. They cannot be joined to metrics from other tables. This is common in event logs.

## Related docs[​](#related-docs "Direct link to Related docs")

* [Joins](https://docs.getdbt.com/docs/build/join-logic.md)
* [Validations](https://docs.getdbt.com/docs/build/validation.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
