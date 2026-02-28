# contract

When the `contract` configuration is enforced, dbt will ensure that your model's returned dataset exactly matches the attributes you have defined in yaml:

* `name` and `data_type` for every column
* Additional [`constraints`](https://docs.getdbt.com/reference/resource-properties/constraints.md), as supported for this materialization and data platform

This is to ensure that the people querying your model downstream—both inside and outside dbt—have a predictable and consistent set of columns to use in their analyses. Even a subtle change in data type, such as from `boolean` (`true`/`false`) to `integer` (`0`/`1`), could cause queries to fail in surprising ways.

Contracts give you control over how schemas are enforced, whether that’s on a single model or consistently across many models in a project.

<!-- -->

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

**These places support model contracts:**

* `dbt_project.yml` file

* `properties.yml` file

* SQL models

* Models materialized as one of the following:

  <!-- -->

  * `table`
  * `view` — views offer limited support for column names and data types, but not `constraints`
  * `incremental` — with `on_schema_change: append_new_columns` or `on_schema_change: fail`

* Certain data platforms, but the supported and [enforced `constraints`](https://docs.getdbt.com/reference/resource-properties/constraints.md) vary by platform

**These places do *NOT* support model contracts:**

* Python models
* `materialized view` or `ephemeral` — materialized SQL models
* Custom materializations (unless added by the author)
* Models with recursive CTE's in BigQuery
* Other resource types, such as `sources`, `seeds`, `snapshots`, and so on

Refer to the [Examples](https://docs.getdbt.com/reference/resource-configs/contract.md#examples) to see how to apply contracts in your project.

## Data type aliasing[​](#data-type-aliasing "Direct link to Data type aliasing")

dbt uses built-in type aliasing for the `data_type` defined in your YAML. For example, you can specify `string` in your contract, and on Postgres/Redshift, dbt will convert it to `text`. If dbt doesn't recognize the `data_type` name among its known aliases, it will pass it through as-is. This is enabled by default, but you can opt-out by setting `alias_types` to `false`.

Example for disabling:

FOLDER\_NAME/FILE\_NAME.yml

```yml

models:
  - name: my_model
    config:
      contract:
        enforced: true
        alias_types: false  # true by default
```

## Size, precision, and scale[​](#size-precision-and-scale "Direct link to Size, precision, and scale")

When dbt compares data types, it will not compare granular details such as size, precision, or scale. We don't think you should sweat the difference between `varchar(256)` and `varchar(257)`, because it doesn't really affect the experience of downstream queriers. You can accomplish a more-precise assertion by [writing or using a custom test](https://docs.getdbt.com/best-practices/writing-custom-generic-tests.md).

Note that you need to specify a varchar size or numeric scale, otherwise dbt relies on default values. For example, if a `numeric` type defaults to a precision of 38 and a scale of 0, then the numeric column stores 0 digits to the right of the decimal (it only stores whole numbers), which might cause it to fail contract enforcement. To avoid this implicit coercion, specify your `data_type` with a nonzero scale, like `numeric(38, 6)`. dbt Core 1.7 and higher provides a warning if you don't specify precision and scale when providing a numeric data type.

### Examples[​](#examples "Direct link to Examples")

models/dim\_customers.yml

```yml
models:
  - name: dim_customers
    config:
      materialized: table
      contract:
        enforced: true
    columns:
      - name: customer_id
        data_type: int
        constraints:
          - type: not_null
      - name: customer_name
        data_type: string
      - name: non_integer
        data_type: numeric(38,3)
```

Let's say your model is defined as:

models/dim\_customers.sql

```sql
select
  'abc123' as customer_id,
  'My Best Customer' as customer_name
```

When you `dbt run` your model, *before* dbt has materialized it as a table in the database, you will see this error:

```txt
20:53:45  Compilation Error in model dim_customers (models/dim_customers.sql)
20:53:45    This model has an enforced contract that failed.
20:53:45    Please ensure the name, data_type, and number of columns in your contract match the columns in your model's definition.
20:53:45
20:53:45    | column_name | definition_type | contract_type | mismatch_reason    |
20:53:45    | ----------- | --------------- | ------------- | ------------------ |
20:53:45    | customer_id | TEXT            | INT           | data type mismatch |
20:53:45
20:53:45
20:53:45    > in macro assert_columns_equivalent (macros/materializations/models/table/columns_spec_ddl.sql)
```

* Project YAML
* Properties YAML
* SQL file config

Use a contract enforcement in your `dbt_project.yml` to enforce contracts consistently across multiple models:

```yml

models:
  property_management:  # replace with your dbt project name
    +contract:
      enforced: true
```

Define a model’s contract in a `properties.yml` by specifying the expected columns and data types:

```yml

models:
  - name: stg_rental_applications  # replace with your model name
    config:
      contract:
        enforced: true
    columns:
      - name: column_1_id  # example id column. Replace with your column
        data_type: int    # replace with your column's data type
      - name: column_2_created_at  # example column tracking when something was created
        data_type: timestamp
      - name: column_3_status      # example status column, which typically store text values ("active", "pending", "completed", etc.)
        data_type: string
```

Enforce a contract in a model SQL file when you want to apply it to a single model and maintain fine-grained control:

```sql

{{ config(
  contract = { "enforced": true }  -- Enables contract enforcement for this model
) }}

select
  column_1_id,          -- replace with your column
  column_2_created_at,  -- replace with your column
  column_3_status       -- replace with your column
from {{ source('property_management', 'rental_applications') }}  -- replace with your source name and table
```

Refer to [General configurations](https://docs.getdbt.com/reference/model-configs.md#general-configurations) for more information on the supported configs available for model SQL files, `dbt_project.yml` and `properties.yml`.

### Incremental models and `on_schema_change`[​](#incremental-models-and-on_schema_change "Direct link to incremental-models-and-on_schema_change")

Why require that incremental models also set [`on_schema_change`](https://docs.getdbt.com/docs/build/incremental-models.md#what-if-the-columns-of-my-incremental-model-change), and why to `append_new_columns` or `fail`?

Imagine:

* You add a new column to both the SQL and the YAML spec
* You don't set `on_schema_change`, or you set `on_schema_change: 'ignore'`
* dbt doesn't actually add that new column to the existing table — and the upsert/merge still succeeds, because it does that upsert/merge on the basis of the already-existing "destination" columns only (this is long-established behavior)
* The result is a delta between the yaml-defined contract, and the actual table in the database - which means the contract is now incorrect!

Why `append_new_columns` (or `fail`) rather than `sync_all_columns`? Because removing existing columns is a breaking change for contracted models! `sync_all_columns` works like `append_new_columns` but also removes deleted columns, which you're not supposed to do with contracted models unless you upgrade the version.

## Related documentation[​](#related-documentation "Direct link to Related documentation")

* [What is a model contract?](https://docs.getdbt.com/docs/mesh/govern/model-contracts.md)
* [Defining `columns`](https://docs.getdbt.com/reference/resource-properties/columns.md)
* [Defining `constraints`](https://docs.getdbt.com/reference/resource-properties/constraints.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
