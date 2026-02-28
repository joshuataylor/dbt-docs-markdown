# volatility

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
  - name: <function name>
    config:
      volatility: deterministic | stable | non-deterministic
```

## Definition[​](#definition "Direct link to Definition")

<!-- -->

You can optionally use the [`volatility` config](https://docs.getdbt.com/reference/resource-configs/volatility.md) for SQL or Python UDFs to describe how predictable the function output is by using `deterministic`, `stable`, or `non-deterministic`. Warehouses use this information to decide if results can be cached, reordered, or inlined. Setting the appropriate volatility helps prevent incorrect results when a function isn’t safe to cache or reorder.

For example:

* A function that returns a random number (`random()`) should be set as `non-deterministic` because its output changes every time it’s called.
* A function that returns today’s date (`current_date()`) is `stable`; its value remains consistent within a single query execution but may change between queries. If it were configured as `deterministic`, a warehouse might incorrectly cache the value and reuse it on subsequent days.

By default, dbt does not specify a volatility value. If you don’t set volatility, dbt generates a `CREATE` statement without a volatility keyword, and the warehouse’s default behavior applies — except in Redshift.

In Redshift, dbt sets `non-deterministic` (`VOLATILE`) by default if no volatility is specified, because Redshift requires an explicit volatility and `VOLATILE` is the safest assumption.

<!-- -->

 Warehouse-specific volatility keywords

Different warehouses show volatility controls with different keywords and default values:

| Warehouse                                                                                                                         | `deterministic` | `stable`      | `non-deterministic`                         |
| --------------------------------------------------------------------------------------------------------------------------------- | --------------- | ------------- | ------------------------------------------- |
| [Snowflake](https://docs.snowflake.com/en/sql-reference/sql/create-function#sql-handler)                                          | `IMMUTABLE`     | Not supported | `VOLATILE` (default)                        |
| [Redshift](https://docs.aws.amazon.com/redshift/latest/dg/r_CREATE_FUNCTION.html#r_CREATE_FUNCTION-synopsis)                      | `IMMUTABLE`     | `STABLE`      | `VOLATILE` (default)                        |
| [Databricks](https://docs.databricks.com/aws/en/udf/unity-catalog#set-deterministic-if-your-function-produces-consistent-results) | `DETERMINISTIC` | Not supported | Assumed `non-deterministic` unless declared |
| [Postgres](https://www.postgresql.org/docs/current/xfunc-volatility.html)                                                         | `IMMUTABLE`     | `STABLE`      | `VOLATILE` (default)                        |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

BigQuery does not support explicitly setting volatility. Instead, BigQuery infers volatility based on the functions and expressions used within the UDF.

## Supported volatility types[​](#supported-volatility-types "Direct link to Supported volatility types")

In dbt, you can use the following values for the `volatility` config:

| Value               | Description                                                                                                                                                                                                                                                                                             | Example                                                                                                                                                                       |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `deterministic`     | Always returns the same output for the same input. Safe for aggressive optimizations and caching.                                                                                                                                                                                                       | `substr()` — Produces the same substring when given the same string and parameters.                                                                                           |
| `stable`            | Returns the same value within a single query execution, but may change across executions. Not supported by all warehouses. For more information, see [Warehouse-specific volatility keywords](https://docs.getdbt.com/reference/resource-configs/volatility.md#warehouse-specific-volatility-keywords). | `now()` — Returns the current timestamp the moment a query starts; constant within a single query but different across runs.                                                  |
| `non-deterministic` | May return different results for the same inputs. Warehouses shouldn't cache or reorder assuming stable results.                                                                                                                                                                                        | `first()` — May return different rows depending on query plan or ordering.<br />`random()` — Produces a random number that varies with each call, even with identical inputs. |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Example[​](#example "Direct link to Example")

In this example, we're using the `deterministic` volatility for the `is_positive_int` function:

functions/schema.yml

```yaml
functions:
  - name: is_positive_int
    description: Check whether a string is a positive integer
    config:
      volatility: deterministic # Optional: stable | non-deterministic | deterministic
    arguments:
      - name: a_string
        data_type: string
    returns:
      data_type: boolean
```

## Related documentation[​](#related-documentation "Direct link to Related documentation")

* [User-defined functions](https://docs.getdbt.com/docs/build/udfs.md)
* [Function properties](https://docs.getdbt.com/reference/function-properties.md)
* [Function configurations](https://docs.getdbt.com/reference/function-configs.md)
* [Type](https://docs.getdbt.com/reference/resource-configs/type.md)
* [Arguments](https://docs.getdbt.com/reference/resource-properties/function-arguments.md)
* [Returns](https://docs.getdbt.com/reference/resource-properties/returns.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
