# limit

Limit the number of failures that will be returned by a test query. We recommend using this config when working with large datasets and [storing failures in the database](https://docs.getdbt.com/reference/resource-configs/store_failures.md).

* Specific test
* One-off test
* Generic test block
* Project level

Configure a specific instance of a generic (schema) test:

models/\<filename>.yml

```yaml

models:
  - name: large_table
    columns:
      - name: very_unreliable_column
        data_tests:
          - accepted_values:
              arguments: # available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
                values: ["a", "b", "c"]
              config:
                limit: 1000  # will only include the first 1000 failures
```

Configure a one-off (data) test:

tests/\<filename>.sql

```sql
{{ config(limit = 1000) }}

select ...
```

Set the default for all instances of a generic (schema) test, by setting the config inside its test block (definition):

macros/\<filename>.sql

```sql
{% test <testname>(model, column_name) %}

{{ config(limit = 500) }}

select ...

{% endtest %}
```

Set the default for all tests in a package or project:

dbt\_project.yml

```yaml
data_tests:
  +limit: 1000  # all tests
  
  <package_name>:
    +limit: 50 # tests in <package_name>
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
