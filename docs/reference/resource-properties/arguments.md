# arguments (for macros)

macros/\<filename>.yml

```yml


macros:
  - name: <macro name>
    arguments:
      - name: <arg name>
        type: <string>
        description: <markdown_string>
```

## Definition[​](#definition "Direct link to Definition")

The `arguments` property is used to define the parameters that a resource can accept. Each argument can have a `name`, a `type` field, and an optional `description`.

For **macros**, you can add `arguments` to a [macro property](https://docs.getdbt.com/reference/macro-properties.md), which helps in documenting the macro and understanding what inputs it requires.

## type[​](#type "Direct link to type")

<!-- -->

<!-- -->

tip

From dbt Core v1.10, you can opt into validating the arguments you define in macro documentation using the `validate_macro_args` behavior change flag. When enabled, dbt will:

* Infer arguments from the macro and includes them in the [manifest.json](https://docs.getdbt.com/reference/artifacts/manifest-json.md) file if no arguments are documented.
* Raise a warning if documented argument names don't match the macro definition.
* Raise a warning if `type` fields don't follow [supported formats](https://docs.getdbt.com/reference/resource-properties/arguments.md#supported-types).

Learn more about [macro argument validation](https://docs.getdbt.com/reference/global-configs/behavior-changes.md#macro-argument-validation).

macros/\<filename>.yml

```yml

macros:
  - name: <macro name>
    arguments:
      - name: <arg name>
        type: <string>
```

### Supported types[​](#supported-types "Direct link to Supported types")

From dbt Core v1.10, when you use the [`validate_macro_args`](https://docs.getdbt.com/reference/global-configs/behavior-changes.md#macro-argument-validation) flag, dbt supports the following types for macro arguments:

* `string` or `str`
* `boolean` or `bool`
* `integer` or `int`
* `float`
* `any`
* `list[<Type>]`, for example, `list[string]`
* `dict[<Type>, <Type>]`, for example, `dict[str, list[int]]`
* `optional[<Type>]`, for example, `optional[integer]`
* [`relation`](https://docs.getdbt.com/reference/dbt-classes.md#relation)
* [`column`](https://docs.getdbt.com/reference/dbt-classes.md#column)

Note that the types follow a Python-like style but are used for documentation and validation only. They are not Python types.

## Examples[​](#examples "Direct link to Examples")

macros/cents\_to\_dollars.sql

```sql
{% macro cents_to_dollars(column_name, scale=2) %}
    ({{ column_name }} / 100)::numeric(16, {{ scale }})
{% endmacro %}
```

macros/cents\_to\_dollars.yml

```yml

macros:
  - name: cents_to_dollars
    arguments:
      - name: column_name
        type: column
        description: "The name of a column"
      - name: scale
        type: integer
        description: "The number of decimal places to round to. Default is 2."
```

## Related documentation[​](#related-documentation "Direct link to Related documentation")

* [Macro properties](https://docs.getdbt.com/reference/macro-properties.md)
* [Arguments (for functions)](https://docs.getdbt.com/reference/resource-properties/function-arguments.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
