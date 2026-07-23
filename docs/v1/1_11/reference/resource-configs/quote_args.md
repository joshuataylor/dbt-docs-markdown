# quote\_args

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
      snowflake:
        quote_args: true | false  # optional, default: true
```

## Definition[​](#definition "Direct link to Definition")

When creating JavaScript user-defined functions (UDFs) on Snowflake, `quote_args` controls whether argument names are quoted in the `CREATE FUNCTION` statement. Defaults to `true`.

When `quote_args` is `true`, Snowflake quotes argument names, which preserves their exact casing. Inside the function body, reference arguments using the same case as defined in the YAML (for example, `a_string`).

When `quote_args` is `false`, argument names are not quoted. Snowflake uppercases them automatically, so you must reference them in uppercase inside the function body (for example, `A_STRING`).

This config applies to JavaScript UDFs on Snowflake only. It has no effect on other adapters or languages.

## Example[​](#example "Direct link to Example")

In this example, `quote_args` is set to `false` so argument names are not quoted in the generated `CREATE FUNCTION` statement:

functions/is\_positive\_int.yml

```yaml
functions:
  - name: is_positive_int
    description: Returns 1 if a string represents a positive integer, else 0
    config:
      snowflake:
        quote_args: false
    arguments:
      - name: a_string
        data_type: string
    returns:
      data_type: integer
```

functions/is\_positive\_int.js

```js
return /^[0-9]+$/.test(A_STRING) ? 1 : 0;
```

With `quote_args: false`, dbt generates:

```sql
CREATE OR REPLACE FUNCTION udf_db.udf_schema.is_positive_int(a_string STRING)
RETURNS INTEGER
LANGUAGE JAVASCRIPT
AS $$
return /^[0-9]+$/.test(A_STRING) ? 1 : 0;
$$;
```

With the default `quote_args: true`, dbt generates:

```sql
CREATE OR REPLACE FUNCTION udf_db.udf_schema.is_positive_int("a_string" STRING)
RETURNS INTEGER
LANGUAGE JAVASCRIPT
AS $$
return /^[0-9]+$/.test(a_string) ? 1 : 0;
$$;
```

## Related documentation[​](#related-documentation "Direct link to Related documentation")

* [User-defined functions](https://docs.getdbt.com/docs/build/udfs.md)
* [Function properties](https://docs.getdbt.com/reference/function-properties.md)
* [Function configurations](https://docs.getdbt.com/reference/function-configs.md)
* [volatility](https://docs.getdbt.com/reference/resource-configs/volatility.md)
* [type](https://docs.getdbt.com/reference/resource-configs/type.md)
* [arguments](https://docs.getdbt.com/reference/resource-properties/function-arguments.md)
* [returns](https://docs.getdbt.com/reference/resource-properties/returns.md)
