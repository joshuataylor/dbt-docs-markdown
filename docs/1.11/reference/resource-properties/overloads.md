# overloads [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

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
    arguments:
      - name: <arg name>
        data_type: <string>
    returns:
      data_type: <string>
    overloads:
      - defined_in: <string>       # required, name of the SQL, Python, or JavaScript file
        arguments:                 # optional
          - name: <arg name>       # required if arguments is specified
            data_type: <string>    # required if arguments is specified, warehouse-specific
            description: <markdown_string>
            default_value: <string | boolean | integer> # optional, Snowflake and Postgres only
        returns:                   # optional, inherits from root function if omitted
          data_type: <string>      # required if returns is specified, warehouse-specific
          description: <markdown_string>
      - defined_in: ...            # declare additional overloads
```

## Definition[​](#definition "Direct link to Definition")

Beta feature

The `overloads` property is a beta feature in dbt Core v1.12.

The `overloads` property lets you define multiple argument signatures for the same [user-defined function UDF](https://docs.getdbt.com/docs/build/udfs.md). This lets you call the same function name with different input types, without creating separate UDFs for each variant. The warehouse calls the right version based on the argument types. `overloads` is supported for SQL UDFs in Snowflake and Postgres, and Python and JavaScript UDFs in Snowflake.

Each overload references a separate file that contains its function body, with optional `arguments` and `returns`. All overloads are grouped into one DAG node (the root function), so they're built and selected together. On retry, dbt skips overloads that succeeded and reruns only those that failed.

## Behavior[​](#behavior "Direct link to Behavior")

dbt runs all overloads regardless of individual failures, so you get a complete picture of which overloads succeeded and which failed. The following behaviors apply:

* If any overload fails, dbt marks the function node as `PARTIAL_SUCCESS` and skips downstream nodes.
* [`dbt retry`](https://docs.getdbt.com/reference/commands/retry.md) skips overloads that already succeeded and only re-runs the previously failed ones.
* [`state:modified`](https://docs.getdbt.com/reference/node-selection/methods.md#the-state-method) detects changes to any overload's function body, arguments, or return type and marks the root function node as modified.

## Properties[​](#properties "Direct link to Properties")

Each entry in the `overloads` list supports the following properties.

### defined\_in[​](#defined_in "Direct link to defined_in")

The name of the file (without extension) that contains the overload's function body. The file must exist in the `functions/` directory. For example, `defined_in: null_if_empty_numeric` references `functions/null_if_empty_numeric.sql` for SQL UDFs, `functions/null_if_empty_numeric.py` for Python UDFs, or `functions/null_if_empty_numeric.js` for JavaScript UDFs.

Each overload must reference a unique file. The root function's file and all `defined_in` values must be distinct.

dbt raises a parsing error if:

* A `defined_in` value matches the root function's own file.
* Two overloads reference the same file.
* The referenced file doesn't exist in the `functions/` directory.

### arguments[​](#arguments "Direct link to arguments")

The argument list for the overload. Follows the same structure as [function arguments](https://docs.getdbt.com/reference/resource-properties/function-arguments.md).

### returns[​](#returns "Direct link to returns")

The return type for the overload. Follows the same structure as [returns](https://docs.getdbt.com/reference/resource-properties/returns.md). If omitted, the overload inherits the return type of the root function.

## Example[​](#example "Direct link to Example")

* SQL
* Python
* JavaScript

functions/null\_if\_empty.yml

```yml
functions:
  - name: null_if_empty
    arguments:
      - name: val
        data_type: varchar
    returns:
      data_type: varchar
    overloads:
      - defined_in: null_if_empty_numeric
        arguments:
          - name: val
            data_type: numeric
        returns:
          data_type: numeric
```

Create a separate SQL file for each overload body. In this example, the base function handles empty strings, and the overload handles numeric values:

functions/null\_if\_empty.sql

```sql
# syntax for Snowflake
CASE WHEN val = '' THEN NULL ELSE val END

# syntax for Postgres
SELECT CASE WHEN val = '' THEN NULL ELSE val END
```

functions/null\_if\_empty\_numeric.sql

```sql
# syntax for Snowflake
CASE WHEN val = 0 THEN NULL ELSE val END

# syntax for Postgres
SELECT CASE WHEN val = 0 THEN NULL ELSE val END
```

functions/null\_if\_empty.yml

```yml
functions:
  - name: null_if_empty
    config:
      runtime_version: "3.11"
      entry_point: main
    arguments:
      - name: val
        data_type: varchar
    returns:
      data_type: varchar
    overloads:
      - defined_in: null_if_empty_numeric
        arguments:
          - name: val
            data_type: numeric
        returns:
          data_type: numeric
```

Create a separate Python file for each overload body. In this example, the base function handles empty strings, and the overload handles numeric values:

functions/null\_if\_empty.py

```python
def main(val):
    return None if val == '' else val
```

functions/null\_if\_empty\_numeric.py

```python
def main(val):
    return None if val == 0 else val
```

functions/null\_if\_empty.yml

```yml
functions:
  - name: null_if_empty
    arguments:
      - name: val
        data_type: varchar
    returns:
      data_type: varchar
    overloads:
      - defined_in: null_if_empty_numeric
        arguments:
          - name: val
            data_type: numeric
        returns:
          data_type: numeric
```

Create a separate JavaScript file for each overload body. In this example, the base function handles empty strings, and the overload handles numeric values:

functions/null\_if\_empty.js

```javascript
if (val === '') return null;
return val;
```

functions/null\_if\_empty\_numeric.js

```javascript
if (val === 0) return null;
return val;
```

## Related documentation[​](#related-documentation "Direct link to Related documentation")

* [User-defined functions](https://docs.getdbt.com/docs/build/udfs.md)
* [Function properties](https://docs.getdbt.com/reference/function-properties.md)
* [Function arguments](https://docs.getdbt.com/reference/resource-properties/function-arguments.md)
* [returns](https://docs.getdbt.com/reference/resource-properties/returns.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
