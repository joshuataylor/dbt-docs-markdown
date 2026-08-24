# runtime\_version

Did you know\...

Available from dbt v1.11 or with the [dbt "Latest" release track](../../docs/dbt-versions/dbt-release-tracks.md).

functions/\<filename>.yml

```yml
functions:
  - name: <function name>
    config:
      runtime_version: <string> # required for Snowflake and BigQuery; optional and ignored on Databricks
```

## Definition

When creating Python UDFs, specify the Python version to run in `runtime_version`.

## Supported values

* [Snowflake](https://docs.snowflake.com/en/developer-guide/udf/python/udf-python-introduction): `3.10`, `3.11`, `3.12`, and `3.13`
* [BigQuery](https://cloud.google.com/bigquery/docs/user-defined-functions-python): `3.11`
* Databricks: Accepted for compatibility but has no effect. Databricks manages the Python runtime internally, so dbt displays a warning if you set it. Available in `dbt-databricks` v1.12+.

## Example

In this example, we're using the Python version `3.11` for the UDF.

functions/schema.yml

```yaml
functions:
  - name: is_positive_int
    config:
      runtime_version: "3.11"
```

## Related documentation

* [User-defined functions](../../docs/build/udfs.md)
* [Function properties](../function-properties.md)
* [Function configurations](../function-configs.md)
* [type](./type.md)
* [volatility](./volatility.md)
* [entry\_point](./entry-point.md)
* [packages](./packages.md)
* [arguments](../resource-properties/function-arguments.md)
* [returns](../resource-properties/returns.md)
