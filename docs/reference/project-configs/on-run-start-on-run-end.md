# on-run-start & on-run-end

dbt\_project.yml

```yml
on-run-start: sql-statement | [sql-statement]
on-run-end: sql-statement | [sql-statement]
```

## Definition[​](#definition "Direct link to Definition")

A SQL statement (or list of SQL statements) to be run at the start or end of the following commands:

`dbt build`, `dbt compile`, `dbt docs generate`, `dbt run`, `dbt seed`, `dbt snapshot`, or `dbt test`.

`on-run-start` and `on-run-end` hooks can also [call macros](#call-a-macro-to-grant-privileges) that return SQL statements.

## Usage notes[​](#usage-notes "Direct link to Usage notes")

* The `on-run-end` hook has additional Jinja variables available in the context — check out the [docs](https://docs.getdbt.com/reference/dbt-jinja-functions/on-run-end-context.md).

## Examples[​](#examples "Direct link to Examples")

### Grant privileges on all schemas that dbt uses at the end of a run[​](#grant-privileges-on-all-schemas-that-dbt-uses-at-the-end-of-a-run "Direct link to Grant privileges on all schemas that dbt uses at the end of a run")

This leverages the [schemas](https://docs.getdbt.com/reference/dbt-jinja-functions/schemas.md) variable that is only available in an `on-run-end` hook.

dbt\_project.yml

```yml
on-run-end:
  - "{% for schema in schemas %}grant usage on schema {{ schema }} to group reporter; {% endfor %}"
```

### Call a macro to grant privileges[​](#call-a-macro-to-grant-privileges "Direct link to Call a macro to grant privileges")

dbt\_project.yml

```yml
on-run-end: "{{ grant_select(schemas) }}"
```

### Additional examples[​](#additional-examples "Direct link to Additional examples")

We've compiled some more in-depth examples [here](https://docs.getdbt.com/docs/build/hooks-operations.md#additional-examples).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
