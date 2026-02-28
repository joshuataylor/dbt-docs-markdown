# runtime\_version

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
      runtime_version: <string> # required for Python UDFs
```

## Definition[​](#definition "Direct link to Definition")

When creating Python UDFs, specify the Python version to run in `runtime_version`.

## Supported values[​](#supported-values "Direct link to Supported values")

* [Snowflake](https://docs.snowflake.com/en/developer-guide/udf/python/udf-python-introduction): `3.10`, `3.11`, `3.12`, and `3.13`
* [BigQuery](https://cloud.google.com/bigquery/docs/user-defined-functions-python): `3.11`

## Example[​](#example "Direct link to Example")

In this example, we're using the Python version `3.11` for the UDF.

functions/schema.yml

```yaml
functions:
  - name: is_positive_int
    config:
      runtime_version: "3.11"
```

## Related documentation[​](#related-documentation "Direct link to Related documentation")

* [User-defined functions](https://docs.getdbt.com/docs/build/udfs.md)
* [Function properties](https://docs.getdbt.com/reference/function-properties.md)
* [Function configurations](https://docs.getdbt.com/reference/function-configs.md)
* [Type](https://docs.getdbt.com/reference/resource-configs/type.md)
* [Volatility](https://docs.getdbt.com/reference/resource-configs/volatility.md)
* [entry\_point](https://docs.getdbt.com/reference/resource-configs/entry-point.md)
* [Arguments](https://docs.getdbt.com/reference/resource-properties/function-arguments.md)
* [Returns](https://docs.getdbt.com/reference/resource-properties/returns.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
