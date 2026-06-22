# packages[Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

💡Did you know\...

Available from dbt v

<!-- -->

1.12

<!-- -->

or with the

<!-- -->

[dbt "Latest" release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md).

functions/\<filename>.yml

```yml
functions:
  - name: <function name>
    config:
      packages: [<string>] # optional, Python UDFs only
```

## Definition[​](#definition "Direct link to Definition")

When creating Python UDFs, you can use the optional `packages` config to specify public third-party PyPI packages. You can list package names (for example, `numpy` and `pandas`) and pin versions (for example, `pandas==1.5.0`). The warehouse installs these packages when it creates the UDF, so your UDF can use functionality from external Python libraries.

On Snowflake, some packages are installed from the Anaconda repository, and you may need to [accept Anaconda's Terms of Service](https://docs.snowflake.com/en/developer-guide/udf/python/udf-python-packages#using-third-party-packages-from-anaconda) before you can use them.

Python UDFs are currently supported in Snowflake and BigQuery. Each warehouse uses a different mechanism to specify packages. For example:

* Snowflake
* BigQuery

functions/schema.yml

```yaml
functions:
  - name: is_positive_int
    description: Returns 1 if a_string matches ^[0-9]+$, else 0
    config:
      runtime_version: "3.11"
      entry_point: main
      packages:
        - numpy
        - pandas==1.5.0
    arguments:
      - name: a_string
        data_type: string
    returns:
      data_type: integer
```

Compiled SQL:

```sql
CREATE OR REPLACE FUNCTION my_schema.is_positive_int(a_string STRING)
  RETURNS INTEGER
  LANGUAGE PYTHON
  RUNTIME_VERSION = '3.11'
  HANDLER = 'main'
  PACKAGES = ('numpy','pandas==1.5.0')
AS $$
def main(a_string):
    ...
$$
```

functions/schema.yml

```yaml
functions:
  - name: is_positive_int
    description: Returns 1 if a_string matches ^[0-9]+$, else 0
    config:
      runtime_version: "3.11"
      entry_point: main
      packages:
        - numpy
        - pandas==1.5.0
    arguments:
      - name: a_string
        data_type: string
    returns:
      data_type: integer
```

Compiled SQL:

```sql
CREATE OR REPLACE FUNCTION my_dataset.is_positive_int(a_string STRING)
  RETURNS INT64
  LANGUAGE python
  OPTIONS(
    runtime_version = "python-3.11",
    entry_point = "main",
    packages = ['numpy', 'pandas==1.5.0']
  )
AS r'''
def main(a_string):
    ...
'''
```

## Related documentation[​](#related-documentation "Direct link to Related documentation")

* [User-defined functions](https://docs.getdbt.com/docs/build/udfs.md)
* [Function properties](https://docs.getdbt.com/reference/function-properties.md)
* [Function configurations](https://docs.getdbt.com/reference/function-configs.md)
* [runtime\_version](https://docs.getdbt.com/reference/resource-configs/runtime-version.md)
* [entry\_point](https://docs.getdbt.com/reference/resource-configs/entry-point.md)
* [type](https://docs.getdbt.com/reference/resource-configs/type.md)
* [volatility](https://docs.getdbt.com/reference/resource-configs/volatility.md)
* [arguments](https://docs.getdbt.com/reference/resource-properties/function-arguments.md)
* [returns](https://docs.getdbt.com/reference/resource-properties/returns.md)
