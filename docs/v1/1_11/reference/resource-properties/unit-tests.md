# About unit tests property

💡Did you know\...

Available from dbt v

<!-- -->

1.8

<!-- -->

or with the

<!-- -->

[dbt "Latest" release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md).

Unit tests validate your SQL modeling logic on a small set of static inputs before you materialize your full model in production. They support a test-driven development approach, improving both the efficiency of developers and reliability of code.

To run only your unit tests, use the command: `dbt test --select test_type:unit`

<!-- -->

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* We currently only support unit testing SQL models.
* We currently only support adding unit tests to models in your *current* project.
* We currently *don't* support unit testing models that use the [`materialized view`](https://docs.getdbt.com/docs/build/materializations.md#materialized-view) materialization.
* We currently *don't* support unit testing models that use recursive SQL.
* We currently *don't* support unit testing models that use introspective queries.
* If your model has multiple versions, by default the unit test will run on *all* versions of your model. Read [unit testing versioned models](https://docs.getdbt.com/reference/resource-properties/unit-testing-versions.md) for more information.
* Unit tests must be defined in a YML file in your [`models/` directory](https://docs.getdbt.com/reference/project-configs/model-paths.md).
* Table names must be aliased in order to unit test `join` logic.
* Include all [`ref`](https://docs.getdbt.com/reference/dbt-jinja-functions/ref.md) or [`source`](https://docs.getdbt.com/reference/dbt-jinja-functions/source.md) model references in the unit test configuration as `input`s to avoid "node not found" errors during compilation.

Unit tests are discovered from `model-paths` (by default, the `models/` directory), so define them alongside your models in a `.yml` file under your `model-paths`. Don't define unit test YAML in the `tests/` directory, which is reserved for [data tests](https://docs.getdbt.com/docs/build/data-tests.md).

models/schema.yml

```yaml

unit_tests:
  - name: <test-name> # this is the unique name of the test
    model: <model-name>
    versions: #optional
      include: <list-of-versions-to-include> #optional
      exclude: <list-of-versions-to-exclude> #optional
    config: 
      meta: {dictionary}
      tags: <string> | [<string>]
      enabled: {boolean} # optional. v1.9 or higher. If not configured, defaults to `true`
    given:
      - input: <ref_or_source_call> # optional for seeds
        format: dict | csv | sql
        # either define rows inline or name of fixture
        rows: {dictionary} | <string>
        fixture: <fixture-name> # SQL or csv 
      - input: ... # declare additional inputs
    expect:
      format: dict | csv | sql
      # either define rows inline or use the name of a fixture
      rows: {dictionary} | <string>
      fixture: <fixture-name> # SQL or csv 
    overrides: # optional: configuration for the dbt execution environment
      macros:
        is_incremental: true | false
        dbt_utils.current_timestamp: <string>
        # ... any other Jinja function from https://docs.getdbt.com/reference/dbt-jinja-functions
        # ... any other context property
      vars: {dictionary}
      env_vars: {dictionary}
  - name: <test-name> ... # declare additional unit tests
```

## Examples[​](#examples "Direct link to Examples")

models/schema.yml

```yaml

unit_tests:
  - name: test_is_valid_email_address # this is the unique name of the test
    model: dim_customers # name of the model I'm unit testing
    given: # the mock data for your inputs
      - input: ref('stg_customers')
        rows:
         - {email: cool@example.com,     email_top_level_domain: example.com}
         - {email: cool@unknown.com,     email_top_level_domain: unknown.com}
         - {email: badgmail.com,         email_top_level_domain: gmail.com}
         - {email: missingdot@gmailcom,  email_top_level_domain: gmail.com}
      - input: ref('top_level_email_domains')
        rows:
         - {tld: example.com}
         - {tld: gmail.com}
    expect: # the expected output given the inputs above
      rows:
        - {email: cool@example.com,    is_valid_email_address: true}
        - {email: cool@unknown.com,    is_valid_email_address: false}
        - {email: badgmail.com,        is_valid_email_address: false}
        - {email: missingdot@gmailcom, is_valid_email_address: false}
```

models/schema.yml

```yaml

unit_tests:
  - name: test_is_valid_email_address # this is the unique name of the test
    model: dim_customers # name of the model I'm unit testing
    given: # the mock data for your inputs
      - input: ref('stg_customers')
        rows:
         - {email: cool@example.com,     email_top_level_domain: example.com}
         - {email: cool@unknown.com,     email_top_level_domain: unknown.com}
         - {email: badgmail.com,         email_top_level_domain: gmail.com}
         - {email: missingdot@gmailcom,  email_top_level_domain: gmail.com}
      - input: ref('top_level_email_domains')
        format: csv
        rows: |
          tld
          example.com
          gmail.com
    expect: # the expected output given the inputs above
      format: csv
      fixture: valid_email_address_fixture_output
```

models/schema.yml

```yaml

unit_tests:
  - name: test_is_valid_email_address # this is the unique name of the test
    model: dim_customers # name of the model I'm unit testing
    given: # the mock data for your inputs
      - input: ref('stg_customers')
        rows:
         - {email: cool@example.com,     email_top_level_domain: example.com}
         - {email: cool@unknown.com,     email_top_level_domain: unknown.com}
         - {email: badgmail.com,         email_top_level_domain: gmail.com}
         - {email: missingdot@gmailcom,  email_top_level_domain: gmail.com}
      - input: ref('top_level_email_domains')
        format: sql
        rows: |
          select 'example.com' as tld union all
          select 'gmail.com' as tld
    expect: # the expected output given the inputs above
      format: sql
      fixture: valid_email_address_fixture_output
```
