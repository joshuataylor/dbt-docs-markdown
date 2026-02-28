# About dbt run-operation command

### Overview[​](#overview "Direct link to Overview")

The `dbt run-operation` command is used to invoke a macro. For usage information, consult the docs on [operations](https://docs.getdbt.com/docs/build/hooks-operations.md#about-operations).

### Usage[​](#usage "Direct link to Usage")

```text
$ dbt run-operation {macro} --args '{args}'
  {macro}        Specify the macro to invoke. dbt will call this macro
                        with the supplied arguments and then exit
  --args ARGS           Supply arguments to the macro. This dictionary will be
                        mapped to the keyword arguments defined in the
                        selected macro. This argument should be a YAML string,
                        eg. '{my_variable: my_value}'
```

### Command line examples[​](#command-line-examples "Direct link to Command line examples")

Example 1:

`$ dbt run-operation grant_select --args '{role: reporter}'`

Example 2:

`$ dbt run-operation clean_stale_models --args '{days: 7, dry_run: True}'`

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
