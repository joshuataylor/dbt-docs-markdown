# returns

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
    returns:
      data_type: <string> # required, warehouse-specific
      description: <markdown_string> # optional
```

## Definition[​](#definition "Direct link to Definition")

The `returns` property defines the output of a user-defined function (UDF). This is a required property for all functions and specifies what data type the function will return when executed.

## Properties[​](#properties "Direct link to Properties")

### data\_type (required)[​](#data_type-required "Direct link to data_type (required)")

The `data_type` field specifies the data type that the function returns. This is a required field and must match the data types supported by your specific data platform.

Warehouse-specific data types

The `data_type` values are warehouse-specific. Use the data type syntax that your warehouse requires:

* **Snowflake**: `STRING`, `NUMBER`, `BOOLEAN`, `TIMESTAMP_NTZ`, `VARIANT`, etc.
* **BigQuery**: `STRING`, `INT64`, `BOOL`, `TIMESTAMP`, `ARRAY<STRING>`, `STRUCT`, etc.
* **Redshift**: `VARCHAR`, `INTEGER`, `BOOLEAN`, `TIMESTAMP`, `SUPER`, etc.
* **Postgres**: `TEXT`, `INTEGER`, `BOOLEAN`, `TIMESTAMP`, `JSONB`, etc.

Refer to your warehouse documentation for the complete list of supported data types and their syntax.

### description[​](#description "Direct link to description")

An optional markdown string describing what the function returns. This is helpful for documentation purposes.

## Examples[​](#examples "Direct link to Examples")

### Simple scalar function[​](#simple-scalar-function "Direct link to Simple scalar function")

functions/schema.yml

```yml

functions:
  - name: is_valid_email
    description: Validates if a string is a properly formatted email address
    arguments:
      - name: email_string
        data_type: STRING
        description: The email address to validate
    returns:
      data_type: BOOLEAN
      description: Returns true if the string is a valid email format, false otherwise
```

### Function with complex return type[​](#function-with-complex-return-type "Direct link to Function with complex return type")

functions/schema.yml

```yml

functions:
  - name: calculate_metrics
    description: Calculates revenue and profit metrics
    arguments:
      - name: revenue
        data_type: DECIMAL(18,2)
      - name: cost
        data_type: DECIMAL(18,2)
    returns:
      data_type: DECIMAL(18,2)
      description: The calculated profit margin as a percentage
```

### BigQuery function with ARRAY return type[​](#bigquery-function-with-array-return-type "Direct link to BigQuery function with ARRAY return type")

functions/schema.yml

```yml

functions:
  - name: split_tags
    description: Splits a comma-separated string into an array of tags
    arguments:
      - name: tag_string
        data_type: STRING
    returns:
      data_type: ARRAY<STRING>
      description: An array of individual tag strings
```

## Related documentation[​](#related-documentation "Direct link to Related documentation")

* [User-defined functions](https://docs.getdbt.com/docs/build/udfs.md)
* [Function properties](https://docs.getdbt.com/reference/function-properties.md)
* [Function configurations](https://docs.getdbt.com/reference/function-configs.md)
* [Arguments](https://docs.getdbt.com/reference/resource-properties/function-arguments.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
