# Model contracts

## Related documentation

* [`contract`](../../../reference/resource-configs/contract.md)
* [`columns`](../../../reference/resource-properties/columns.md)
* [`constraints`](../../../reference/resource-properties/constraints.md)

## Why define a contract?

Defining a dbt model is as easy as writing a SQL `select` statement. Your query naturally produces a dataset with columns of names and types based on the columns you select and the transformations you apply.

While this is ideal for quick and iterative development, for some models, constantly changing the shape of its returned dataset poses a risk when other people and processes are querying that model. It's better to define a set of upfront "guarantees" that define the shape of your model. We call this set of guarantees a "contract." While building your model, dbt will verify that your model's transformation will produce a dataset matching up with its contract, or it will fail to build.

#### Considerations

There are some considerations to keep in mind when using model governance features:

* Model governance features like model access, contracts, and versions strengthen trust and stability in your dbt project. Because they add structure, they can make rollbacks harder (for example, removing model access) and increase maintenance if adopted too early. Before adding governance features, consider whether your dbt project is ready to benefit from them. Introducing governance while models are still changing can complicate future changes.

* Governance features are model-specific. They don't apply to other resource types, including snapshots, seeds, or sources. This is because these objects can change structure over time (for example, snapshots capture evolving historical data) and aren't suited to guarantees like contracts, access, or versioning.

## Prerequisites

**These places support model contracts:**

* `dbt_project.yml` file

* `properties.yml` file

* SQL models

* Models materialized as one of the following:

  * `table`
  * `view` — views offer support for column names and data types, but not `constraints`
  * `incremental` — with `on_schema_change: append_new_columns` or `on_schema_change: fail`

* Certain data platforms, but the supported and [enforced `constraints`](../../../reference/resource-properties/constraints.md) vary by platform

**These places do *NOT* support model contracts:**

* Python models
* `materialized view` or `ephemeral` — materialized SQL models
* Custom materializations (unless added by the author)
* Models with recursive CTE's in BigQuery
* Other resource types, such as `sources`, `seeds`, `snapshots`, and so on

## Define a contract

Let's say you have a model with a query like:

models/marts/dim\_customers.sql

```sql
-- lots of SQL

final as (

    select
        customer_id,
        customer_name,
        -- ... many more ...
    from ...

)

select * from final
```

To enforce a model's contract, set `enforced: true` under the `contract` configuration.

When enforced, your contract *must* include every column's `name` and `data_type` (where `data_type` matches one that your data platform understands).

If your model is materialized as `table` or `incremental`, and depending on your data platform, you may optionally specify additional [constraints](../../../reference/resource-properties/constraints.md), such as `not_null` (containing zero null values).

models/marts/customers.yml

```yaml
models:
  - name: dim_customers
    config:
      contract:
        enforced: true
    columns:
      - name: customer_id
        data_type: int
        constraints:
          - type: not_null
      - name: customer_name
        data_type: string
      ...
```

When building a model with a defined contract, dbt will do two things differently:

1. dbt will run a "preflight" check to ensure that the model's query will return a set of columns with names and data types matching the ones you have defined. This check is agnostic to the order of columns specified in your model (SQL) or YAML spec.
2. dbt will include the column names, data types, and constraints in the DDL statements it submits to the data platform, which will be enforced while building or updating the model's table, and order the columns per the contract instead of your dbt model.

## Platform constraint support

Select the adapter-specific tab for more information on [constraint](../../../reference/resource-properties/constraints.md) support across platforms. Constraints fall into three categories based on definability and platform enforcement:

* **Definable and enforced** — The model won't build if it violates the constraint.
* **Definable and not enforced** — The platform supports specifying the type of constraint, but a model can still build even if building the model violates the constraint. This constraint exists for metadata purposes only. This approach is more typical in cloud data warehouses than in transactional databases, where strict rule enforcement is more common.
* **Not definable and not enforced** — You can't specify the type of constraint for the platform.

### Redshift

| Constraint type | Definable | Enforced |
| --------------- | --------- | -------- |
| not\_null       | ✅        | ✅       |
| primary\_key    | ✅        | ❌       |
| foreign\_key    | ✅        | ❌       |
| unique          | ✅        | ❌       |
| check           | ❌        | ❌       |

### Snowflake

| Constraint type | Definable | Enforced |
| --------------- | --------- | -------- |
| not\_null       | ✅        | ✅       |
| primary\_key    | ✅        | ❌       |
| foreign\_key    | ✅        | ❌       |
| unique          | ✅        | ❌       |
| check           | ❌        | ❌       |

### BigQuery

| Constraint type | Definable | Enforced |
| --------------- | --------- | -------- |
| not\_null       | ✅        | ✅       |
| primary\_key    | ✅        | ❌       |
| foreign\_key    | ✅        | ❌       |
| unique          | ❌        | ❌       |
| check           | ❌        | ❌       |

### Postgres

| Constraint type | Definable | Enforced |
| --------------- | --------- | -------- |
| not\_null       | ✅        | ✅       |
| primary\_key    | ✅        | ✅       |
| foreign\_key    | ✅        | ✅       |
| unique          | ✅        | ✅       |
| check           | ✅        | ✅       |

### Spark

Currently, `not_null` and `check` constraints are enforced only after a model is built. Because of this platform limitation, dbt considers these constraints definable but not enforced, which means they're not part of the *model contract* since they can't be enforced at build time. This table will change as the features evolve.

| Constraint type | Definable | Enforced |
| --------------- | --------- | -------- |
| not\_null       | ✅        | ❌       |
| primary\_key    | ✅        | ❌       |
| foreign\_key    | ✅        | ❌       |
| unique          | ✅        | ❌       |
| check           | ✅        | ❌       |

### Databricks

Currently, `not_null` and `check` constraints are enforced only after a model is built. Because of this platform limitation, dbt considers these constraints definable but not enforced, which means they're not part of the *model contract* since they can't be enforced at build time. This table will change as the features evolve.

| Constraint type | Definable | Enforced |
| --------------- | --------- | -------- |
| not\_null       | ✅        | ✅       |
| primary\_key    | ✅        | ❌       |
| foreign\_key    | ✅        | ❌       |
| unique          | ❌        | ❌       |
| check           | ✅        | ✅       |

### Fabric

Microsoft Fabric creates the table first, then adds some constraints in separate steps. Only `not_null` is checked when the model builds. Primary key, foreign key, and unique constraints are recorded on the table for reference, but Fabric does not check them when new data is added. Define `primary_key`, `foreign_key`, and `unique` at the model level. The adapter ignores those constraint types at the column level.

| Constraint type | Definable | Enforced |
| --------------- | --------- | -------- |
| not\_null       | ✅        | ✅       |
| primary\_key    | ✅        | ❌       |
| foreign\_key    | ✅        | ❌       |
| unique          | ✅        | ❌       |
| check           | ❌        | ❌       |

### Athena

| Constraint type | Definable | Enforced |
| --------------- | --------- | -------- |
| not\_null       | ❌        | ❌       |
| primary\_key    | ❌        | ❌       |
| foreign\_key    | ❌        | ❌       |
| unique          | ❌        | ❌       |
| check           | ❌        | ❌       |

## FAQs

 Which models should have contracts?

Any model meeting the criteria described above *can* define a contract. We recommend defining contracts for ["public" models](./model-access.md) that are being relied on downstream.

* Inside of dbt: Shared with other groups, other teams, and [other dbt projects](../../../best-practices/how-we-mesh/mesh-1-intro.md).
* Outside of dbt: Reports, dashboards, or other systems & processes that expect this model to have a predictable structure. You might reflect these downstream uses with [exposures](../../build/exposures.md).

 How are contracts different from tests?

A model's contract defines the **shape** of the returned dataset. If the model's logic or input data doesn't conform to that shape, the model does not build.

[Data Tests](../../build/data-tests.md) are a more flexible mechanism for validating the content of your model *after* it's built. So long as you can write the query, you can run the data test. Data tests are more configurable, such as with [custom severity thresholds](../../../reference/resource-configs/severity.md). They are easier to debug after finding failures because you can query the already-built model, or [store the failing records in the data warehouse](../../../reference/resource-configs/store_failures.md).

In some cases, you can replace a data test with its equivalent constraint. This has the advantage of guaranteeing the validation at build time, and it probably requires less compute (cost) in your data platform. The prerequisites for replacing a data test with a constraint are:

* Making sure that your data platform can support and enforce the constraint that you need. Most platforms only enforce `not_null`.
* Materializing your model as `table` or `incremental` (**not** `view` or `ephemeral`).
* Defining a full contract for this model by specifying the `name` and `data_type` of each column.

**Why aren't tests part of the contract?** In a parallel for software APIs, the structure of the API response is the contract. Quality and reliability ("uptime") are also very important attributes of an API's quality, but they are not part of the contract per se. When the contract changes in a backwards-incompatible way, it is a breaking change that requires a bump in major version.

 Do I need to define every column for a contract?

Yes. dbt contracts apply to *all* columns defined in a model, and they require declaring explicit expectations about *all* of those columns. The explicit declaration of a contract is not an accident — it's very much the intent of this feature.

At the same time, for models with many columns, we understand that this can mean a *lot* of YAML. See [dbt-labs/dbt#11764](https://github.com/dbt-labs/dbt/issues/11764) for discussion of potential approaches to generate and update model contract definitions.

 How are breaking changes handled?

When comparing to a previous project state, dbt will look for breaking changes that could impact downstream consumers. If breaking changes are detected, dbt will present a contract error.

Breaking changes include:

* Removing an existing column
* Changing the data\_type of an existing column
* Removing or modifying one of the `constraints` on an existing column (dbt v1.6 or higher)
* Removing a contracted model by deleting, renaming, or disabling it (dbt v1.9 or higher).
  * versioned models will raise an error. unversioned models will raise a warning.

More details are available in the [contract reference](../../../reference/resource-configs/contract.md#incremental-models-and-on_schema_change).

 Can I use model contracts with nested fields?

Yes, but only on BigQuery, because nested fields inside [struct columns](https://docs.cloud.google.com/bigquery/docs/reference/standard-sql/data-types#struct_type) are strongly typed. Model contracts are useful when a model contains nested JSON or event data, where a nested field that changes type could break downstream models.

Nested fields (structs) are columns inside other columns. List each inner field in YAML with a dotted name, such as `b.id` or `b.double_nested.another.even_more`. You must declare a `name` and `data_type` for every inner field. You may optionally declare the struct column (in this example, `b` or `b.double_nested`) in order to add a description or a constraint on the struct.

models/nested\_fields.yml

```yaml
models:
  - name: double_nested_fields
    config:
      contract:
        enforced: true
      materialized: table
    columns:
      - name: a
        data_type: string
      - name: b.id
        data_type: integer
      - name: b.name
        data_type: string
      - name: b.double_nested.id
        data_type: integer
      - name: b.double_nested.another.again
        data_type: string
      - name: b.double_nested.another.even_more
        data_type: integer
```

models/double\_nested\_fields.sql

```sql
select
  'string' as a,
  struct(
    1 as id,
    'name' as name,
    struct(2 as id, struct('test' as again, 2 as even_more) as another) as double_nested
  ) as b
```

When every nested field's `name` and `data_type` match the SQL, the model builds as usual. If they do not match, the contract fails and the model does not build. For example, if `even_more` is a string in SQL but `integer` in the contract, dbt returns:

```text
This model has an enforced contract that failed.
Please ensure the name, data_type, and number of columns in your contract match the columns in your model's definition.

| column_name                       | definition_type | contract_type | mismatch_reason    |
| --------------------------------- | --------------- | ------------- | ------------------ |
| b.double_nested.another.even_more | STRING          | INT64         | data type mismatch |
```

Arrays of structs are supported too. Declare the parent with `data_type: array`, then list each inner field:

```yaml
- name: my_array
  data_type: array
- name: my_array.id
  data_type: integer
- name: my_array.name
  data_type: string
```

To add constraints such as `not_null` on nested fields, refer to [Constraints](../../../reference/resource-properties/constraints.md).
