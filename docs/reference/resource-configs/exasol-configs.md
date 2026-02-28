# Exasol configurations

## Incremental materialization strategies[​](#incremental-materialization-strategies "Direct link to Incremental materialization strategies")

In dbt-exasol, the following incremental materialization strategies are supported:

* `append` (default when `unique_key` is not defined)
* `merge`
* `delete+insert` (default when `unique_key` is defined)
* [`microbatch`](https://docs.getdbt.com/docs/build/incremental-microbatch.md)

All of these strategies are inherited from dbt-core. For more information on incremental strategies, refer to the [incremental strategy documentation](https://docs.getdbt.com/docs/build/incremental-strategy.md).

## Performance optimizations[​](#performance-optimizations "Direct link to Performance optimizations")

### Table distribution and partitioning[​](#table-distribution-and-partitioning "Direct link to Table distribution and partitioning")

Starting from dbt-exasol 1.8.1, you can configure table distribution and partitioning strategies to optimize query performance in Exasol. These configurations are available for models materialized as `table` or `incremental`.

Exasol supports the following performance optimization configurations:

| Parameter              | Type                       | Required | Description                                                                |
| ---------------------- | -------------------------- | -------- | -------------------------------------------------------------------------- |
| `partition_by_config`  | `<string>` or `[<string>]` | no       | Partitions the table by specified column(s) for improved query performance |
| `distribute_by_config` | `<string>`                 | no       | Distributes data across cluster nodes by specified column                  |
| `primary_key_config`   | `[<string>]`               | no       | Defines primary key constraint(s)                                          |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

* Project YAML file
* Properties YAML file
* SQL file config

dbt\_project.yml

```yaml
models:
  <resource-path>:
    +materialized: table
    +partition_by_config: <column-name>
    +distribute_by_config: <column-name>
    +primary_key_config: [<column-name>]
```

models/properties.yml

```yaml
models:
  - name: [<model-name>]
    config:
      materialized: table
      partition_by_config: <column-name>
      distribute_by_config: <column-name>
      primary_key_config: [<column-name>]
```

models/\<model\_name>.sql

```jinja
{{ config(
    materialized="table",
    partition_by_config="<column-name>",
    distribute_by_config="<column-name>",
    primary_key_config=["<column-name>"]
) }}
```

#### Single column example[​](#single-column-example "Direct link to Single column example")

The following example creates a table partitioned by `order_date`, distributed by `customer_id`, with a primary key on `customer_id`:

models/orders.sql

```sql
{{
    config(
        materialized='table',
        primary_key_config=['customer_id'],
        partition_by_config='order_date',
        distribute_by_config='customer_id'
    )
}}

select
    customer_id,
    order_date,
    order_total,
    order_status
from {{ source('sales', 'orders') }}
```

#### Multiple columns example[​](#multiple-columns-example "Direct link to Multiple columns example")

When configuring multiple columns for `primary_key_config`, provide them as a list:

models/order\_items.sql

```sql
{{
    config(
        materialized='incremental',
        primary_key_config=['order_id', 'item_id'],
        partition_by_config='order_date',
        distribute_by_config='order_id',
        unique_key=['order_id', 'item_id']
    )
}}

select
    order_id,
    item_id,
    order_date,
    quantity,
    price
from {{ source('sales', 'order_items') }}
```

info

When configuring multiple columns for `primary_key_config`, always provide them as a list: `['column1', 'column2']`

For more information about Exasol's table distribution and partitioning, refer to the [Exasol documentation](https://docs.exasol.com/db/latest/sql/create_table.htm).

## Model contracts[​](#model-contracts "Direct link to Model contracts")

Exasol supports [model contracts](https://docs.getdbt.com/docs/mesh/govern/model-contracts.md) with the following database constraints:

| Constraint Type | Support Status   | Description                                 |
| --------------- | ---------------- | ------------------------------------------- |
| `not_null`      | ✅ Enforced      | Prevents NULL values in the column          |
| `primary_key`   | ✅ Enforced      | Enforces uniqueness and NOT NULL            |
| `foreign_key`   | ✅ Enforced      | References another table's primary key      |
| `check`         | ❌ Not supported | Custom validation expressions not supported |
| `unique`        | ❌ Not supported | Unique constraints not supported            |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### Example with enforced constraints[​](#example-with-enforced-constraints "Direct link to Example with enforced constraints")

models/customers.yml

```yaml
models:
  - name: customers
    config:
      contract:
        enforced: true
    columns:
      - name: customer_id
        data_type: integer
        constraints:
          - type: not_null
          - type: primary_key
      - name: email
        data_type: varchar(255)
        constraints:
          - type: not_null
      - name: country_id
        data_type: integer
        constraints:
          - type: foreign_key
            expression: countries (country_id)
```

For more information on model contracts, refer to the [model contracts documentation](https://docs.getdbt.com/docs/mesh/govern/model-contracts.md).

## Timestamp format[​](#timestamp-format "Direct link to Timestamp format")

Starting from dbt-exasol 1.2.2, the default timestamp format is `YYYY-MM-DDTHH:MI:SS.FF6`.

You can customize the timestamp format in your [profile configuration](https://docs.getdbt.com/docs/core/connect-data-platform/exasol-setup.md):

profiles.yml

```yaml
outputs:
  dev:
    type: exasol
    timestamp_format: 'YYYY-MM-DD HH24:MI:SS.FF3'
    # ... other settings
```

### Microbatch strategy considerations[​](#microbatch-strategy-considerations "Direct link to Microbatch strategy considerations")

When using the [`microbatch`](https://docs.getdbt.com/docs/build/incremental-microbatch.md) incremental strategy, Exasol requires timestamps without timezone suffix in model definitions:

```sql
-- ✅ Correct (Exasol compatible)
TIMESTAMP '2024-01-01 10:00:00'

-- ❌ Incorrect (will cause parse errors)
TIMESTAMP '2024-01-01 10:00:00-0'
```

The dbt-exasol adapter automatically handles timestamp formatting for microbatch boundaries.

For more information about the microbatch strategy, refer to the [microbatch documentation](https://docs.getdbt.com/docs/build/incremental-microbatch.md).

## Materialized views[​](#materialized-views "Direct link to Materialized views")

Exasol does not support materialized views. If you attempt to use `materialized='materialized_view'`, the operation will fail with an error.

### Workarounds[​](#workarounds "Direct link to Workarounds")

* Use `materialized='table'` with appropriate refresh logic
* Use `materialized='incremental'` with suitable [incremental strategies](https://docs.getdbt.com/docs/build/incremental-strategy.md)

## Clone operations[​](#clone-operations "Direct link to Clone operations")

Exasol does not support table cloning operations. This affects dbt features that rely on `CLONE` functionality.

## Unit test limitations[​](#unit-test-limitations "Direct link to Unit test limitations")

Exasol has specific limitations with [unit tests](https://docs.getdbt.com/docs/build/unit-tests.md):

### Empty string handling[​](#empty-string-handling "Direct link to Empty string handling")

In Exasol, empty strings are treated as `NULL`. This affects test fixtures that use empty string literals to simulate empty values. When writing unit tests with seed data, be aware that:

```yaml
# This seed data
id,name,value
1,test,""  # Empty string

# Will be interpreted as
id,name,value
1,test,NULL  # NULL value in Exasol
```

### Cross-database testing[​](#cross-database-testing "Direct link to Cross-database testing")

Unit tests that rely on sources in a database different from the models are not supported. All test fixtures and models must exist in the same database.

### Aggregate functions in CTEs[​](#aggregate-functions-in-ctes "Direct link to Aggregate functions in CTEs")

Exasol does not support certain aggregate functions (`LISTAGG`, `MEDIAN`, `PERCENTILE_CONT`) when used within common table expressions (CTEs) created from dbt's unit test fixtures. These functions require user-created tables.

**Workaround:** Create actual tables for test fixtures rather than using inline CTEs when testing models with these functions.

If you are interested in supporting materialized test fixtures, we encourage you to participate in this issue in GitHub: [dbt-labs/dbt-core#8499](https://github.com/dbt-labs/dbt-core/issues/8499)

## Connection configuration[​](#connection-configuration "Direct link to Connection configuration")

For information about connection parameters such as encryption, SSL/TLS validation, OpenID authentication, and other profile settings, refer to the [Exasol setup documentation](https://docs.getdbt.com/docs/core/connect-data-platform/exasol-setup.md).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
