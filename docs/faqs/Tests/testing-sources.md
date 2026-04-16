# How do I run data tests on just my sources?


To run data tests on all sources, use the following command:

```shell
  dbt test --select "source:*"
```

(You can also use the `-s` shorthand here instead of `--select`)

To run data tests on one source (and all of its tables):

```shell
$ dbt test --select source:jaffle_shop
```

And, to run data tests on one source <Term id="table" /> only:

```shell
$ dbt test --select source:jaffle_shop.orders
```

