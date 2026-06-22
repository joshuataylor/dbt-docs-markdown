# About on-run-end context variable

Caution

These variables are only available in the context for `on-run-end` hooks. They will evaluate to `none` if used outside of an `on-run-end` hook!

## schemas[​](#schemas "Direct link to schemas")

The `schemas` context variable can be used to reference the schemas that dbt has built models into during a run of dbt. This variable can be used to grant usage on these schemas to certain users at the end of a dbt run.

Example:

dbt\_project.yml

```sql

on-run-end:
 - "{% for schema in schemas %}grant usage on schema {{ schema }} to db_reader;{% endfor %}"
```

In practice, it might not be a bad idea to put this code into a macro:

macros/grants.sql

```jinja2

{% macro grant_usage_to_schemas(schemas, user) %}
  {% for schema in schemas %}
    grant usage on schema {{ schema }} to {{ user }};
  {% endfor %}
{% endmacro %}
```

dbt\_project.yml

```yaml

on-run-end:
 - "{{ grant_usage_to_schemas(schemas, 'user') }}"
```

## database\_schemas[​](#database_schemas "Direct link to database_schemas")

The `database_schemas` context variable can be used to reference the databases *and* schemas that dbt has built models into during a run of dbt. This variable is similar to the `schemas` variable, and should be used if a dbt run builds resources into multiple different databases.

Example:

macros/grants.sql

```jinja2

{% macro grant_usage_to_schemas(database_schemas, user) %}
  {% for (database, schema) in database_schemas %}
    grant usage on {{ database }}.{{ schema }} to {{ user }};
  {% endfor %}
{% endmacro %}
```

dbt\_project.yml

```yaml

on-run-end:
 - "{{ grant_usage_to_schemas(database_schemas, user) }}"
```

## Results[​](#results "Direct link to Results")

The `results` variable contains a list of [Result objects](https://docs.getdbt.com/reference/dbt-classes.md#result-objects) with one element per resource that executed in the dbt job. The Result object provides access within the Jinja on-run-end context to the information that will populate the [run results JSON artifact](https://docs.getdbt.com/reference/artifacts/run-results-json.md).

Example usage:

macros/log\_results.sql

```sql
{% macro log_results(results) %}

  {% if execute %}
  {{ log("========== Begin Summary ==========", info=True) }}
  {% for res in results -%}
    {% set line -%}
        node: {{ res.node.unique_id }}; status: {{ res.status }} (message: {{ res.message }})
    {%- endset %}

    {{ log(line, info=True) }}
  {% endfor %}
  {{ log("========== End Summary ==========", info=True) }}
  {% endif %}

{% endmacro %}
```

dbt\_project.yml

```yaml

on-run-end: "{{ log_results(results) }}"
```

Results:

```text
12:48:17 | Concurrency: 1 threads (target='dev')
12:48:17 |
12:48:17 | 1 of 2 START view model dbt_jcohen.abc............................... [RUN]
12:48:17 | 1 of 2 OK created view model dbt_jcohen.abc.......................... [CREATE VIEW in 0.11s]
12:48:17 | 2 of 2 START table model dbt_jcohen.def.............................. [RUN]
12:48:17 | 2 of 2 ERROR creating table model dbt_jcohen.def..................... [ERROR in 0.09s]
12:48:17 |
12:48:17 | Running 1 on-run-end hook
========== Begin Summary ==========
node: model.testy.abc; status: success (message: CREATE VIEW)
node: model.testy.def; status: error (message: Database Error in model def (models/def.sql)
  division by zero
  compiled SQL at target/run/testy/models/def.sql)
========== End Summary ==========
12:48:17 | 1 of 1 START hook: testy.on-run-end.0................................ [RUN]
12:48:17 | 1 of 1 OK hook: testy.on-run-end.0................................... [OK in 0.00s]
12:48:17 |
12:48:17 |
12:48:17 | Finished running 1 view model, 1 table model, 1 hook in 1.94s.
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
