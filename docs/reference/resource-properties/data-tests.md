# About data tests property

* Models
* Sources
* Seeds
* Snapshots
* Analyses

models/\<filename>.yml

```yml

models:
  - name: <model_name>
    data_tests:
      - <test_name>:
          arguments: # available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
            <argument_name>: <argument_value>
          config:
            <test_config>: <config-value>

    columns:
      - name: <column_name>
        data_tests:
          - <test_name>
          - <test_name>:
              arguments:
                <argument_name>: <argument_value>
              config:
                <test_config>: <config-value>
```

models/\<filename>.yml

```yml

sources:
  - name: <source_name>
    tables:
    - name: <table_name>
      data_tests:
        - <test_name>
        - <test_name>:
            arguments: # available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
              <argument_name>: <argument_value>
            config:
              <test_config>: <config-value>

      columns:
        - name: <column_name>
          data_tests:
            - <test_name>
            - <test_name>:
                arguments:
                  <argument_name>: <argument_value>
                config:
                  <test_config>: <config-value>
```

seeds/\<filename>.yml

```yml

seeds:
  - name: <seed_name>
    data_tests:
      - <test_name>
      - <test_name>:
          arguments: # available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
            <argument_name>: <argument_value>
          config:
            <test_config>: <config-value>

    columns:
      - name: <column_name>
        data_tests:
          - <test_name>
          - <test_name>:
              arguments:
                <argument_name>: <argument_value>
              config:
                <test_config>: <config-value>
```

snapshots/\<filename>.yml

```yml

snapshots:
  - name: <snapshot_name>
    data_tests:
      - <test_name>
      - <test_name>:
          arguments: # available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
            <argument_name>: <argument_value>
          config:
            <test_config>: <config-value>

    columns:
      - name: <column_name>
        data_tests:
          - <test_name>
          - <test_name>:
              arguments:
                <argument_name>: <argument_value>
              config:
                <test_config>: <config-value>
```

This feature is not implemented for analyses.

## Related documentation[​](#related-documentation "Direct link to Related documentation")

* [Data testing guide](https://docs.getdbt.com/docs/build/data-tests.md)

## Description[​](#description "Direct link to Description")

The `data_tests` property defines assertions about a column, table, or view. The property contains a list of [generic data tests](https://docs.getdbt.com/docs/build/data-tests.md#generic-data-tests), referenced by name, which can include the four built-in generic tests available in dbt. For example, you can add data tests that ensure a column contains no duplicates and zero null values. Any arguments or [configurations](https://docs.getdbt.com/reference/data-test-configs.md) passed to those data tests should be nested below the `arguments` property.

Once these data tests are defined, you can validate their correctness by running `dbt test`.

## Out-of-the-box data tests[​](#out-of-the-box-data-tests "Direct link to Out-of-the-box data tests")

There are four generic data tests that are available out of the box, for everyone using dbt.

### `not_null`[​](#not_null "Direct link to not_null")

This data test validates that there are no `null` values present in a column.

models/\<filename>.yml

```yaml

models:
  - name: orders
    columns:
      - name: order_id
        data_tests:
          - not_null
```

### `unique`[​](#unique "Direct link to unique")

This data test validates that there are no duplicate values present in a field.

The config and where clause are optional.

models/\<filename>.yml

```yaml

models:
  - name: orders
    columns:
      - name: order_id
        data_tests:
          - unique:
              config:
                where: "order_id > 21"
```

### `accepted_values`[​](#accepted_values "Direct link to accepted_values")

This data test validates that all of the non-`null` values in a column are present in a supplied list of `values`. If any values other than those provided in the list are present, then the data test will fail.

The `accepted_values` test supports an optional `quote` parameter which, by default, will single-quote the list of accepted values in the test query. To test non-strings (like integers or boolean values) explicitly set the `quote` config to `false`.

schema.yml

```yaml

models:
  - name: orders
    columns:
      - name: status
        data_tests:
          - accepted_values:
              arguments: # available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
                values: ['placed', 'shipped', 'completed', 'returned']

      - name: status_id
        data_tests:
          - accepted_values:
              arguments:
                values: [1, 2, 3, 4]
                quote: false
```

### `relationships`[​](#relationships "Direct link to relationships")

This data test validates that all of the records in a child table have a corresponding record in a parent table. This property is referred to as "referential integrity". This test automatically excludes `NULL` values from validation, consistent with how database foreign key constraints work. Use the `not_null` test separately if `NULL` values should cause failures.

The following example tests that every order's `customer_id` maps back to a valid `customer`.

schema.yml

```yaml

models:
  - name: orders
    columns:
      - name: customer_id
        data_tests:
          - relationships:
              arguments: # available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
                to: ref('customers')
                field: id
```

The `to` argument accepts a [Relation](https://docs.getdbt.com/reference/dbt-classes.md#relation) – this means you can pass it a `ref` to a model (e.g. `ref('customers')`), or a `source` (e.g. `source('jaffle_shop', 'customers')`).

## Additional examples[​](#additional-examples "Direct link to Additional examples")

### Test an expression[​](#test-an-expression "Direct link to Test an expression")

Some data tests require multiple columns, so it doesn't make sense to nest them under the `columns:` key. In this case, you can apply the data test to the model (or source, seed, or snapshot) instead:

models/orders.yml

```yaml

models:
  - name: orders
    description: 
        Order overview data mart, offering key details for each order including if it's a customer's first order and a food vs. drink item breakdown. One row per order.
    data_tests:
      - dbt_utils.expression_is_true:
          arguments: # available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
            expression: "order_items_subtotal = subtotal"
      - dbt_utils.expression_is_true:
          arguments:
            expression: "order_total = subtotal + tax_paid"
```

This example focuses on testing expressions to ensure that `order_items_subtotal` equals `subtotal` and `order_total` correctly sums `subtotal` and `tax_paid`.

### Use custom generic data test[​](#use-custom-generic-data-test "Direct link to Use custom generic data test")

If you've defined your own custom generic data test, you can use that as the `test_name`:

models/\<filename>.yml

```yaml

models:
  - name: orders
    columns:
      - name: order_id
        data_tests:
          - primary_key  # name of my custom generic test
```

Check out the guide on writing a [custom generic data test](https://docs.getdbt.com/best-practices/writing-custom-generic-tests.md) for more information.

### Custom data test name[​](#custom-data-test-name "Direct link to Custom data test name")

By default, dbt will synthesize a name for your generic data test by concatenating:

* test name (`not_null`, `unique`, etc)
* model name (or source/seed/snapshot)
* column name (if relevant)
* arguments (if relevant, e.g. `values` for `accepted_values`)

It does not include any configurations for the data test. If the concatenated name is too long, dbt will use a truncated and hashed version instead. The goal is to preserve unique identifiers for all resources in your project, including tests.

You may also define your own name for a specific data test, via the `name` property.

**When might you want this?** dbt's default approach can result in some wonky (and ugly) data test names. By defining a custom name, you get full control over how the data test will appear in log messages and metadata artifacts. You'll also be able to select the data test by that name.

models/\<filename>.yml

```yaml

models:
  - name: orders
    columns:
      - name: status
        data_tests:
          - accepted_values:
              name: unexpected_order_status_today
              arguments: # available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
                values: ['placed', 'shipped', 'completed', 'returned']
              config:
                where: "order_date = current_date"
```

```sh
$ dbt test --select unexpected_order_status_today
12:43:41  Running with dbt=1.1.0
12:43:41  Found 1 model, 1 test, 0 snapshots, 0 analyses, 167 macros, 0 operations, 1 seed file, 0 sources, 0 exposures, 0 metrics
12:43:41
12:43:41  Concurrency: 5 threads (target='dev')
12:43:41
12:43:41  1 of 1 START test unexpected_order_status_today ................................ [RUN]
12:43:41  1 of 1 PASS unexpected_order_status_today ...................................... [PASS in 0.03s]
12:43:41
12:43:41  Finished running 1 test in 0.13s.
12:43:41
12:43:41  Completed successfully
12:43:41
12:43:41  Done. PASS=1 WARN=0 ERROR=0 SKIP=0 TOTAL=1
```

A data test's name must be unique for all tests defined on a given model-column combination. If you give the same name to data tests defined on several different columns, or across several different models, then `dbt test --select <repeated_custom_name>` will select them all.

**When might you need this?** In cases where you have defined the same data test twice, with only a difference in configuration, dbt will consider these data tests to be duplicates:

models/\<filename>.yml

```yaml

models:
  - name: orders
    columns:
      - name: status
        data_tests:
          - accepted_values:
              arguments: # available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
                values: ['placed', 'shipped', 'completed', 'returned']
              config:
                where: "order_date = current_date"
          - accepted_values:
              arguments:
                values: ['placed', 'shipped', 'completed', 'returned']
              config:
                # only difference is in the 'where' config
                where: "order_date = (current_date - interval '1 day')" # PostgreSQL syntax
```

```sh
Compilation Error
  dbt found two tests with the name "accepted_values_orders_status__placed__shipped__completed__returned" defined on column "status" in "models.orders".

  Since these resources have the same name, dbt will be unable to find the correct resource
  when running tests.

  To fix this, change the name of one of these resources:
  - test.testy.accepted_values_orders_status__placed__shipped__completed__returned.69dce9e5d5 (models/one_file.yml)
  - test.testy.accepted_values_orders_status__placed__shipped__completed__returned.69dce9e5d5 (models/one_file.yml)
```

By providing a custom name, you help dbt differentiate data tests:

models/\<filename>.yml

```yaml

models:
  - name: orders
    columns:
      - name: status
        data_tests:
          - accepted_values:
              name: unexpected_order_status_today
              arguments: # available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
                values: ['placed', 'shipped', 'completed', 'returned']
              config:
                where: "order_date = current_date"
          - accepted_values:
              name: unexpected_order_status_yesterday
              arguments:
                values: ['placed', 'shipped', 'completed', 'returned']
              config:
                where: "order_date = (current_date - interval '1 day')" # PostgreSQL
```

```sh
$ dbt test
12:48:03  Running with dbt=1.1.0-b1
12:48:04  Found 1 model, 2 tests, 0 snapshots, 0 analyses, 167 macros, 0 operations, 1 seed file, 0 sources, 0 exposures, 0 metrics
12:48:04
12:48:04  Concurrency: 5 threads (target='dev')
12:48:04
12:48:04  1 of 2 START test unexpected_order_status_today ................................ [RUN]
12:48:04  2 of 2 START test unexpected_order_status_yesterday ............................ [RUN]
12:48:04  1 of 2 PASS unexpected_order_status_today ...................................... [PASS in 0.04s]
12:48:04  2 of 2 PASS unexpected_order_status_yesterday .................................. [PASS in 0.04s]
12:48:04
12:48:04  Finished running 2 tests in 0.21s.
12:48:04
12:48:04  Completed successfully
12:48:04
12:48:04  Done. PASS=2 WARN=0 ERROR=0 SKIP=0 TOTAL=2
```

**If using [`store_failures`](https://docs.getdbt.com/reference/resource-configs/store_failures.md):** dbt uses each data test's name as the name of the table in which to store any failing records. If you have defined a custom name for one data test, that custom name will also be used for its table of failures. You may optionally configure an [`alias`](https://docs.getdbt.com/reference/resource-configs/alias.md) for the data test, to separately control both the name of the data test (for metadata) and the name of its database table (for storing failures).

### Alternative format for defining tests[​](#alternative-format-for-defining-tests "Direct link to Alternative format for defining tests")

When defining a generic data test with several arguments and configurations, the YAML can look and feel unwieldy. If you find it easier, you can define the same data test properties as top-level keys of a single dictionary, by providing the data test name as `test_name` instead. It's totally up to you.

This example is identical to the one above:

models/\<filename>.yml

```yaml

models:
  - name: orders
    columns:
      - name: status
        data_tests:
          - name: unexpected_order_status_today
            test_name: accepted_values  # name of the generic test to apply
            arguments: # available in v1.10.5 and higher. Older versions can set the <argument_name> as the top-level property.
              values:
                - placed
                - shipped
                - completed
                - returned
            config:
              where: "order_date = current_date"
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
