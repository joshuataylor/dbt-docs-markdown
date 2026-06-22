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

And, to run data tests on one source table only:

```shell
$ dbt test --select source:jaffle_shop.orders
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
