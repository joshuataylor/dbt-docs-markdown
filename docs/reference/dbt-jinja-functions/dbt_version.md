# About dbt\_version variable

The `dbt_version` variable returns the installed version of dbt that is currently running. It can be used for debugging or auditing purposes. For details about release versioning, refer to [Versioning](https://docs.getdbt.com/reference/commands/version.md#versioning).

## Example usages[​](#example-usages "Direct link to Example usages")

macros/get\_version.sql

```sql
{% macro get_version() %}

  {% do log("The installed version of dbt is: " ~ dbt_version, info=true) %}

{% endmacro %}
```

```text
$ dbt run-operation get_version
The installed version of dbt is 1.6.0
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
