# type

💡Did you know\...

Available from dbt v

<!-- -->

1.11

<!-- -->

or with the

<!-- -->

[dbt "Latest" release track](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md).

functions/\<filename>.yml

```yml
functions:
  - name: function_name
    config:
      type: scalar | aggregate 
```

In the future, we're considering adding support for `table` type. Refer to [this issue](https://github.com/dbt-labs/dbt-core/issues/11917) to track the progress and provide any feedback.

## Definition[​](#definition "Direct link to Definition")

The `type` config specifies the type of user-defined function (UDF) you're creating. This config is optional and defaults to `scalar` if not specified.

## Supported function types[​](#supported-function-types "Direct link to Supported function types")

The following function types are supported:

* [scalar (default)](#scalar-default)
* [aggregate](#aggregate)

Support for `type` differs based on the warehouse and language (SQL or Python) you're using:

| Adapter        | scalar SQL | scalar Python | aggregate SQL | aggregate Python |
| -------------- | ---------- | ------------- | ------------- | ---------------- |
| dbt-bigquery   | ✅         | ✅            | ✅            | ❌               |
| dbt-snowflake  | ✅         | ✅            | ❌            | ✅               |
| dbt-databricks | ✅         | ❌            | ❌            | ❌               |
| dbt-postgres   | ✅         | ❌            | ❌            | ❌               |
| dbt-redshift   | ✅         | ❌            | ❌            | ❌               |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

### scalar (default)[​](#scalar-default "Direct link to scalar (default)")

A scalar function returns a single value for each row of input. This is the most common type of UDF.

**Example use cases:**

* Data validation (checking if a string matches a pattern)
* Data transformation (converting formats, cleaning strings)
* Custom calculations (complex mathematical operations)

functions/schema.yml

```yml
functions:
  - name: is_positive_int
    description: Determines if a string represents a positive integer
    config:
      type: scalar
    arguments:
      - name: input_string
        data_type: STRING
    returns:
      data_type: BOOLEAN
```

### aggregate[​](#aggregate "Direct link to aggregate")

Aggregate functions operate on multiple rows and return a single value — for example, they sum values or calculate an average for a group. Queries use these functions in `GROUP BY` operations.

Aggregate functions are currently supported only for:

* Python functions on Snowflake
* SQL functions on BigQuery

**Example use cases:**

* Calculating totals or averages for groups of data (for example, total sales per customer)
* Aggregating data over time (for example, daily, monthly, or yearly totals)

functions/schema.yml

```yml
functions:
  - name: double_total
    description: Sums values and doubles the result
    config:
      type: aggregate
    arguments:
      - name: values
        data_type: FLOAT
        description: A sequence of numbers to aggregate
    returns:
      data_type: FLOAT
```

## Related documentation[​](#related-documentation "Direct link to Related documentation")

* [User-defined functions](https://docs.getdbt.com/docs/build/udfs.md)
* [Function properties](https://docs.getdbt.com/reference/function-properties.md)
* [Function configurations](https://docs.getdbt.com/reference/function-configs.md)
* [Volatility](https://docs.getdbt.com/reference/resource-configs/volatility.md)
* [Arguments](https://docs.getdbt.com/reference/resource-properties/function-arguments.md)
* [Returns](https://docs.getdbt.com/reference/resource-properties/returns.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
