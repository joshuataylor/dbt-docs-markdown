# function-paths

dbt\_project.yml

```yml
function-paths: [directorypath]
```

## Definition[​](#definition "Direct link to Definition")

Optionally specify a custom list of directories where [user-defined functions (UDFs)](https://docs.getdbt.com/docs/build/udfs.md) are located.

## Default[​](#default "Direct link to Default")

By default, dbt will search for functions in the `functions` directory, for example, `function-paths: ["functions"]`

## Examples[​](#examples "Direct link to Examples")

Use a subdirectory named `udfs` instead of `functions`:

dbt\_project.yml

```yml
function-paths: ["udfs"]
```

Use multiple directories to organize your functions:

dbt\_project.yml

```yml
function-paths: ["functions", "custom_udfs"]
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
