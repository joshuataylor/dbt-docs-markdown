# Data test configurations

## Related documentation

* [Data tests](../docs/build/data-tests.md)

Data tests can be configured in a few different ways:

1. Properties within `.yml` definition (generic tests only, see [test properties](./resource-properties/data-tests.md) for full syntax)
2. A `config()` block within the test's SQL definition
3. In `dbt_project.yml`

Data test configs are applied hierarchically, in the order of specificity outlined above. In the case of a singular test, the `config()` block within the SQL definition takes precedence over configs in the project YAML file. In the case of a specific instance of a generic test, the test's `.yml` properties would take precedence over any values set in its generic SQL definition's `config()`, which in turn would take precedence over values set in the project YAML file (`dbt_project.yml`).

## Available configurations

Click the link on each configuration option to read more about what it can do.

### Data test-specific configurations

Resource-specific configurations are applicable to only one dbt resource type rather than multiple resource types. You can define these settings in the project file (`dbt_project.yml`), a property file (`models/properties.yml` for models, similarly for other resources), or within the resource’s file using the `{{ config() }}` macro.<br />

The following resource-specific configurations are only available to Data tests:

### Project file

dbt\_project.yml

```yaml
data_tests:
  <resource-path>:
    +fail_calc: <string>
    +limit: <integer>
    +severity: error | warn
    +error_if: <string>
    +warn_if: <string>
    +store_failures: true | false
    +where: <string>
```

### SQL file config

```jinja

{{ config(
    fail_calc = "<string>",
    limit = <integer>,
    severity = "error | warn",
    error_if = "<string>",
    warn_if = "<string>",
    store_failures = true | false,
    where = "<string>"
) }}
```

### Property file

```yaml
<resource_type>:
  - name: <resource_name>
    data_tests:
      - <test_name>: # # Actual name of the test. For example, dbt_utils.equality
          name: # Human friendly name for the test. For example, equality_fct_test_coverage
          description: "markdown formatting"
          arguments: # Available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
            <argument_name>: <argument_value>
          config:
            fail_calc: <string>
            limit: <integer>
            severity: error | warn
            error_if: <string>
            warn_if: <string>
            store_failures: true | false
            where: <string>
            # Available in v1.12 and higher. Requires enabling the `require_sql_header_in_test_configs` flag.
            sql_header: <string> 

    columns:
      - name: <column_name>
        data_tests:
          - <test_name>:
              name:
              description: "markdown formatting"
              arguments: # Available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
                <argument_name>: <argument_value>
              config:
                fail_calc: <string>
                limit: <integer>
                severity: error | warn
                error_if: <string>
                warn_if: <string>
                store_failures: true | false
                where: <string>
                # Available in v1.12 and higher. Requires enabling the `require_sql_header_in_test_configs` flag.
                sql_header: <string> 
```

This configuration mechanism is supported for specific instances of generic tests only. To configure a specific singular test, you should use the `config()` macro in its SQL definition.

Starting in dbt Core v1.12, you can set [`sql_header`](./resource-configs/sql_header.md) in the `config` of a generic data test at the model or column level of your `properties.yml`. Enable the [`require_sql_header_in_test_configs`](./global-configs/behavior-flags/require_sql_header_in_test_configs.md) flag to use `config.sql_header` in your data tests.

### General configurations

General configurations provide broader operational settings applicable across multiple resource types. Like resource-specific configurations, these can also be set in the project file, property files, or within resource-specific files.

### Project file

dbt\_project.yml

```yaml
data_tests:
  <resource-path>:
    +enabled: true | false
    +tags: <string> | [<string>]
    +meta: {dictionary}
    # relevant for store_failures only
    +database: <string>
    +schema: <string>
    +alias: <string>
```

### SQL file config

```jinja

{{ config(
    enabled=true | false,
    tags="<string>" | ["<string>"]
    meta={dictionary},
    database="<string>",
    schema="<string>",
    alias="<string>",
) }}
```

### Property file

```yaml

<resource_type>:
  - name: <resource_name>
    data_tests:
      - <test_name>: # Actual name of the test. For example, dbt_utils.equality
          name: # Human friendly name for the test. For example, equality_fct_test_coverage
          description: "markdown formatting"
          arguments: # available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
            <argument_name>: <argument_value>
          config:
            enabled: true | false
            tags: <string> | [<string>]
            meta: {dictionary}
            # relevant for store_failures only
            database: <string>
            schema: <string>
            alias: <string>

    columns:
      - name: <column_name>
        data_tests:
          - <test_name>:
              name: 
              description: "markdown formatting"
              arguments: # available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
                <argument_name>: <argument_value>
              config:
                enabled: true | false
                tags: <string> | [<string>]
                meta: {dictionary}
                # relevant for store_failures only
                database: <string>
                schema: <string>
                alias: <string>
```

This configuration mechanism is supported for specific instances of generic data tests only. To configure a specific singular test, you should use the `config()` macro in its SQL definition.

### Examples

#### Add a tag to one test

If a specific instance of a generic data test:

models/\<filename>.yml

```yml
models:
  - name: my_model
    columns:
      - name: id
        data_tests:
          - unique:
              config:
                tags: ['my_tag'] # changed to config in v1.10
```

If a singular data test:

tests/\<filename>.sql

```sql
{{ config(tags = ['my_tag']) }}

select ...
```

#### Set the default severity for all instances of a generic data test

macros/\<filename>.sql

```sql
{% test my_test() %}

    {{ config(severity = 'warn') }}

    select ...

{% endtest %}
```

#### Disable all data tests from a package

dbt\_project.yml

```yml
data_tests:
  package_name:
    +enabled: false
```

#### Specify custom configurations for generic data tests

Beginning in dbt v1.9, you can use any custom config key to specify custom configurations for data tests. For example, the following specifies the `snowflake_warehouse` custom config that dbt should use when executing the `accepted_values` data test:

```yml

models:
  - name: my_model
    columns:
      - name: color
        data_tests:
          - accepted_values:
              arguments: # available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
                values: ['blue', 'red']
              config:
                severity: warn
                snowflake_warehouse: my_warehouse
```

Given the config, the data test runs on a different Snowflake virtual warehouse than the one in your default connection to enable better price-performance with a different warehouse size or more granular cost allocation and visibility.

#### Add a description to generic and singular tests

Starting from dbt v1.9 (also available to dbt [release tracks](../docs/dbt-versions/dbt-release-tracks.md)), you can add [descriptions](./resource-properties/data-tests.md#description) to both generic and singular tests.

For a generic test, add the description in line with the existing YAML:

models/staging/\<filename>.yml

```yml

models:
  - name: my_model
    columns:
      - name: delivery_status
        data_tests:
          - accepted_values:
              arguments: # available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
                values: ['delivered', 'pending', 'failed']
              description: "This test checks whether there are unexpected delivery statuses. If it fails, check with logistics team"
```

You can also add descriptions to the Jinja macro that provides the core logic of a generic data test. Refer to the [Add description to generic data test logic](../best-practices/writing-custom-generic-tests.md#add-description-to-generic-data-test-logic) for more information.

For a singular test, define it in the test's directory:

tests/my\_custom\_test.yml

```yml

data_tests: 
  - name: my_custom_test
    description: "This test checks whether the rolling average of returns is inside of expected bounds. If it isn't, flag to customer success team"
```

For more information refer to [Add a description to a data test](./resource-properties/description.md#add-a-description-to-a-data-test).

(Applies to dbt v1.12 and later)

#### Set `sql_header` in a generic data test

When the [`require_sql_header_in_test_configs`](./global-configs/behavior-flags/require_sql_header_in_test_configs.md) flag is enabled, you can set [`sql_header`](./resource-configs/sql_header.md) in the `config` of a generic data test so that the specified SQL runs before the test executes (for example, to set session parameters or add a comment):

models/properties.yml

```yaml
models:
  - name: orders
    columns:
      - name: order_id
        data_tests:
          - not_null:
              name: not_null_orders_order_id
              config:
                sql_header: "-- SQL_HEADER_TEST_MARKER"
```
