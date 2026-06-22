# About the `--empty` flag

note

The `--empty` flag is not currently available for Python models. If the flag is used with a Python model, it will be ignored.

During dbt development, you might want to validate that your models are semantically correct without the time-consuming cost of building the entire model in the data warehouse. The [`run`](https://docs.getdbt.com/reference/commands/run.md), [`build`](https://docs.getdbt.com/reference/commands/build.md), [`snapshot`](https://docs.getdbt.com/reference/commands/snapshot.md), and [`compile`](https://docs.getdbt.com/reference/commands/compile.md) commands support the `--empty` flag. Starting in dbt Core v1.12, [`seed`](https://docs.getdbt.com/reference/commands/seed.md) also supports the `--empty` flag for building schema-only dry runs. The `--empty` flag limits the refs and sources to zero rows. dbt will still execute the model SQL against the target data warehouse but will avoid expensive reads of input data. This validates dependencies and ensures your models will build properly.

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
