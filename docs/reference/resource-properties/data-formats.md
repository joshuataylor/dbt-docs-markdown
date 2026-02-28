# Supported data formats for unit tests

Currently, mock data for unit testing in dbt supports three formats:

* `dict` (default): Inline dictionary values.
* `csv`: Inline CSV values or a CSV file.
* `sql`: Inline SQL query or a SQL file. Note: For this format you must supply mock data for *all columns*.

## dict[​](#dict "Direct link to dict")

The `dict` data format is the default if no `format` is defined.

`dict` requires an inline dictionary for `rows`:

models/schema.yml

```yml

unit_tests:
  - name: test_my_model
    model: my_model
    given:
      - input: ref('my_model_a')
        format: dict
        rows:
          - {id: 1, name: gerda}
          - {id: 2, name: michelle}    
```

## CSV[​](#csv "Direct link to CSV")

When using the `csv` format, you can use either an inline CSV string for `rows`:

models/schema.yml

```yml

unit_tests:
  - name: test_my_model
    model: my_model
    given:
      - input: ref('my_model_a')
        format: csv
        rows: |
          id,name
          1,gerda
          2,michelle
```

Or, you can provide the name of a CSV file in the `tests/fixtures` directory (or the configured `test-paths` location) of your project for `fixture`:

models/schema.yml

```yml

unit_tests:
  - name: test_my_model
    model: my_model
    given:
      - input: ref('my_model_a')
        format: csv
        fixture: my_model_a_fixture
```

## sql[​](#sql "Direct link to sql")

Using this format:

* Provides more flexibility for the types of data you can unit test
* Allows you to unit test a model that depends on an ephemeral model

However, when using `format: sql` you must supply mock data for *all rows*.

When using the `sql` format, you can use either an inline SQL query for `rows`:

models/schema.yml

```yml

unit_tests:
  - name: test_my_model
    model: my_model
    given:
      - input: ref('my_model_a')
        format: sql
        rows: |
          select 1 as id, 'gerda' as name, null as loaded_at union all
          select 2 as id, 'michelle', null as loaded_at as name
```

Or, you can provide the name of a SQL file in the `tests/fixtures` directory (or the configured `test-paths` location) of your project for `fixture`:

models/schema.yml

```yml

unit_tests:
  - name: test_my_model
    model: my_model
    given:
      - input: ref('my_model_a')
        format: sql
        fixture: my_model_a_fixture
```

**Note:** Jinja is unsupported in SQL fixtures for unit tests.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
