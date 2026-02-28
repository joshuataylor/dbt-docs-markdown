# About as\_bool filter

The `as_bool` Jinja filter will coerce Jinja-compiled output into a boolean value (`True` or `False`), or return an error if it cannot be represented as a bool.

### Usage:[​](#usage "Direct link to Usage:")

In the example below, the `as_bool` filter is used to coerce a Jinja expression to enable or disable a set of models based on the `target`.

dbt\_project.yml

```yml
models:
  my_project:
    for_export:
      enabled: "{{ (target.name == 'prod') | as_bool }}"
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
