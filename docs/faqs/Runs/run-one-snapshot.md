# How do I run one snapshot at a time?

To run one snapshot, use the `--select` flag, followed by the name of the snapshot:

```shell
$ dbt snapshot --select order_snapshot
```

Check out the [model selection syntax documentation](https://docs.getdbt.com/reference/node-selection/syntax.md) for more operators and examples.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
