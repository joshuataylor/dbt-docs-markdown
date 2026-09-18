# How do I run models downstream of one source?

To run models downstream of a source, use the `source:` selector:

```shell
$ dbt run --select source:jaffle_shop+
```

Report incorrect code

(You can also use the `-s` shorthand here instead of `--select`)

To run models downstream of one source table:

```shell
$ dbt run --select source:jaffle_shop.orders+
```

Report incorrect code

Check out the [model selection syntax](../../reference/node-selection/syntax.md) for more examples!
