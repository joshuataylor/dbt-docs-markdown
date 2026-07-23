# How do I run one model at a time?

To run one model, use the `--select` flag (or `-s` flag), followed by the name of the model:

```shell
$ dbt run --select customers
```

Check out the [model selection syntax documentation](https://docs.getdbt.com/reference/node-selection/syntax.md) for more operators and examples.
