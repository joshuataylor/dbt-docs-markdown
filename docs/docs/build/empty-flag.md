# About the `--empty` flag

note

The `--empty` flag is not currently available for Python models. If the flag is used with a Python model, it will be ignored.

During dbt development, you might want to validate that your models are semantically correct without the time-consuming cost of building the entire model in the data warehouse. The [`run`](https://docs.getdbt.com/reference/commands/run.md) and [`build`](https://docs.getdbt.com/reference/commands/build.md) commands support the `--empty` flag for building schema-only dry runs. The `--empty` flag limits the refs and sources to zero rows. dbt will still execute the model SQL against the target data warehouse but will avoid expensive reads of input data. This validates dependencies and ensures your models will build properly.

### Examples[​](#examples "Direct link to Examples")

Run all models in a project while building only the schemas in your development environment:

```text
dbt run --empty
```

Run a specific model:

```text
dbt run --select path/to/your_model --empty
```

dbt will build and execute the SQL, resulting in an empty schema in the data warehouse.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
