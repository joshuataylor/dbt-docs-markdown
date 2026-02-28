# Semantic models

Tip

Use [dbt Copilot](https://docs.getdbt.com/docs/cloud/dbt-copilot.md), available for dbt Enterprise and Enterprise+ accounts, to generate

<!-- -->

semantic models

<!-- -->

in the Studio IDE only.

Semantic models are the foundation for data definition in MetricFlow, which powers the Semantic Layer:

<!-- -->

<!-- -->

<!-- -->

📹 Learn about the dbt Semantic Layer with on-demand video courses!

Explore our [dbt Semantic Layer on-demand course](https://learn.getdbt.com/courses/semantic-layer) to learn how to define and query metrics in your dbt project.

Additionally, dive into mini-courses for querying the dbt Semantic Layer in your favorite tools: [Tableau](https://courses.getdbt.com/courses/tableau-querying-the-semantic-layer), [Excel](https://learn.getdbt.com/courses/querying-the-semantic-layer-with-excel), [Hex](https://courses.getdbt.com/courses/hex-querying-the-semantic-layer), and [Mode](https://courses.getdbt.com/courses/mode-querying-the-semantic-layer).

Here we describe the Semantic model components with examples:

<!-- -->

<!-- -->

## Semantic models components[​](#semantic-models-components "Direct link to Semantic models components")

The complete spec for semantic models is below:

<!-- -->

<!-- -->

The following example displays a complete configuration and detailed descriptions of each field:

<!-- -->

<!-- -->

<!-- -->

<!-- -->

<!-- -->

<!-- -->

### Description[​](#description "Direct link to Description")

Includes important details of the semantic model. This description will primarily be used by other configuration contributors. You can use the pipe operator `(|)` to include multiple lines in the description.

<!-- -->

<!-- -->

<!-- -->

<!-- -->

<!-- -->

### Primary entity[​](#primary-entity "Direct link to Primary entity")

<!-- -->

<!-- -->

You can define a primary entity using the following configs:

<!-- -->

<!-- -->

* Entity types
* Sample config

Here are the types of keys:

* **Primary** — Only one record per row in the table, and it includes every record in the data platform.
* **Unique** — Only one record per row in the table, but it may have a subset of records in the data platform. Null values may also be present.
* **Foreign** — Can have zero, one, or multiple instances of the same record. Null values may also be present.
* **Natural** — A column or combination of columns in a table that uniquely identifies a record based on real-world data. For example, the `sales_person_id` can serve as a natural key in a `sales_person_department` dimension table.

This example shows a semantic model with three entities and their entity types: `transaction` (primary), `order` (foreign), and `user` (foreign).

To reference a desired column, use the actual column name from the model in the `name` parameter. You can also use `name` as an alias to rename the column, and the `expr` parameter to refer to the original column name or a SQL expression of the column.

```yaml
entity:
  - name: transaction
    type: primary
  - name: order
    type: foreign
    expr: id_order
  - name: user
    type: foreign
    expr: substring(id_order FROM 2)
```

You can refer to entities (join keys) in a semantic model using the `name` parameter. Entity names must be unique within a semantic model, and identifier names can be non-unique across semantic models since MetricFlow uses them for [joins](https://docs.getdbt.com/docs/build/join-logic.md).

### Dimensions[​](#dimensions "Direct link to Dimensions")

[Dimensions](https://docs.getdbt.com/docs/build/dimensions.md) are different ways to organize or look at data. They are effectively the group by parameters for metrics. For example, you might group data by things like region, country, or job title.

<!-- -->

<!-- -->

<!-- -->

<!-- -->

<!-- -->

## Dependencies[​](#dependencies "Direct link to Dependencies")

<!-- -->

<!-- -->

## Related docs[​](#related-docs "Direct link to Related docs")

<!-- -->

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
