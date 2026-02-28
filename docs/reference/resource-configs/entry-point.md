# entry\_point

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
      entry_point: <string> # required for Python UDFs
```

## Definition[​](#definition "Direct link to Definition")

When creating Python UDFs, specify the Python function to be called in `entry_point`.

Python UDFs are currently supported in Snowflake and BigQuery. Each warehouse uses a different name for the entry point function. The following table shows what they’re called:

| Warehouse | How `entry_point` is used                           |
| --------- | --------------------------------------------------- |
| Snowflake | Becomes the `HANDLER` name in `LANGUAGE PYTHON UDF` |
| BigQuery  | Becomes the `entry_point` in `OPTIONS(...)`         |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

## Example[​](#example "Direct link to Example")

For example, if you have a Python UDF in `functions/my_function.py` with the following code which uses the function `main` as the entry point:

functions/my\_function.py

```python
import re

def _digits_only(s: str) -> bool:
    return bool(re.search(r'^[0-9]+$', s or ''))

def _to_flag(is_match: bool) -> int:
    return 1 if is_match else 0

def main(a_string: str) -> int:
    """
    This is used as the entry point for the UDF.
    Returns 1 if a_string represents a positive integer (e.g., "10"),
    else 0.
    """
    return _to_flag(_digits_only(a_string))
```

After defining the UDF, you can specify `main` as the `entry_point` in the YAML file. `entry_point: main` points to the `main` function as the entry point for the UDF, while `_digits_only` and `_to_flag` are helper functions.

functions/schema.yml

```yaml
functions:
  - name: is_positive_int
    description: Returns 1 if a_string matches ^[0-9]+$, else 0
    config:
      runtime_version: "3.11"    # required
      entry_point: main          # required: points to the function above
    arguments:
      - name: a_string
        data_type: string
    returns:
      data_type: integer
```

## Related documentation[​](#related-documentation "Direct link to Related documentation")

* [User-defined functions](https://docs.getdbt.com/docs/build/udfs.md)
* [Function properties](https://docs.getdbt.com/reference/function-properties.md)
* [Function configurations](https://docs.getdbt.com/reference/function-configs.md)
* [Type](https://docs.getdbt.com/reference/resource-configs/type.md)
* [Volatility](https://docs.getdbt.com/reference/resource-configs/volatility.md)
* [runtime\_version](https://docs.getdbt.com/reference/resource-configs/runtime-version.md)
* [Arguments](https://docs.getdbt.com/reference/resource-properties/function-arguments.md)
* [Returns](https://docs.getdbt.com/reference/resource-properties/returns.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
