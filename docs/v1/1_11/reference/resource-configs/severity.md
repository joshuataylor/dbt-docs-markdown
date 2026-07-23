# severity, error\_if, and warn\_if

Tests return a number of failures—most often, this is the count of rows returned by the test query, but it could be a [custom calculation](https://docs.getdbt.com/reference/resource-configs/fail_calc.md). Generally, if the number of failures is nonzero, the test returns an error. This makes sense, as test queries are designed to return all the rows you *don't* want: duplicate records, null values, and more.

It's possible to configure tests to return warnings instead of errors, or to make the test status conditional on the number of failures returned. Maybe 1 duplicate record can count as a warning, but 10 duplicate records should count as an error.

The relevant configs are:

* `severity`: `error` or `warn` (default: `error`)
* `error_if`: conditional expression (default: `!=0`)
* `warn_if`: conditional expression (default: `!=0`)

Conditional expressions can be any comparison logic that is supported by your SQL syntax with an integer number of failures: `> 5`, `= 0`, `between 5 and 10`, and so on.

Here's how those play in practice:

* If `severity: error`, dbt will check the `error_if` condition first. If the error condition is met, the test returns an error. If it's not met, dbt will then check the `warn_if` condition (defaulted to `!=0`). If it's not specified or the warn condition is met, the test warns; if it's not met, the test passes.
* If `severity: warn`, dbt will skip the `error_if` condition entirely and jump straight to the `warn_if` condition. If the warn condition is met, the test warns; if it's not met, the test passes.

By default, a test with `severity: warn` will only ever return a warning, and not cause errors. However, you can promote warnings to errors using:

* `--warn-error`: Promotes *all* dbt warnings (including test warnings, Jinja warnings, deprecations, and so on.) to errors.
* `--warn-error-options`: Promotes *only specific types* of warnings.

For more information, refer to [Warnings](https://docs.getdbt.com/reference/global-configs/warnings.md).

* Out-of-the-box generic tests
* Singular tests
* Custom generic tests
* Project level

Configure a specific instance of an out-of-the-box generic test:

models/\<filename>.yml

```yaml

models:
  - name: large_table
    columns:
      - name: slightly_unreliable_column
        data_tests:
          - unique:
              config:
                severity: error
                error_if: ">1000"
                warn_if: ">10"
```

Configure a singular test:

tests/\<filename>.sql

```sql
{{ config(error_if = '>50') }}

select ...
```

Set the default for all instances of a custom generic test, by setting the config inside its test block (definition):

macros/\<filename>.sql

```sql
{% test <testname>(model, column_name) %}

{{ config(severity = 'warn') }}

select ...

{% endtest %}
```

Set the default for all tests in a package or project:

dbt\_project.yml

```yaml
data_tests:
  +severity: warn  # all tests

  <package_name>:
    +warn_if: >10 # tests in <package_name>
```

### Asserting an expected failure[​](#asserting-an-expected-failure "Direct link to Asserting an expected failure")

You can use `error_if` to assert an expected failure. This is useful in package integration tests, for example when validating that a generic test catches known-bad fixture data.

In the following example, the test passes only when it returns one or more failing rows. If the test returns `0` rows, dbt raises an error because `0` satisfies `error_if: '<1'`.

models/always\_bad.yml

```yaml
models:
  - name: always_bad
    data_tests:
      - dbt_utils.expression_is_true:
          arguments:
            expression: "amount > 0"
          config:
            error_if: "<1"
            warn_if: "<0"
```

In this example, the test passes only when it returns one or more failing rows. If the test returns `0` rows, dbt raises an error because `0` satisfies `error_if: '<1'`.

Set `warn_if: '<0'` to take warning behavior out of play for this pattern. Without overriding `warn_if`, the default warning condition (`!=0`) can still apply.
