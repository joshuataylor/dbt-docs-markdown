# How do I build one seed at a time?

You can use a `--select` option with the `dbt seed` command, like so:

```shell

$ dbt seed --select country_codes
```

There is also an `--exclude` option.

Check out more in the [model selection syntax](https://docs.getdbt.com/reference/node-selection/syntax.md) documentation.
