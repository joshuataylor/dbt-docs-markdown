# How do I build one seed at a time?

You can use a `--select` option with the `dbt seed` command, like so:

```shell

$ dbt seed --select country_codes
```

There is also an `--exclude` option.

Check out more in the [model selection syntax](https://docs.getdbt.com/reference/node-selection/syntax.md) documentation.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
