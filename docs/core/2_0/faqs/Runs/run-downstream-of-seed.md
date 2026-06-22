# How do I run models downstream of a seed?

You can run models downstream of a seed using the [model selection syntax](https://docs.getdbt.com/reference/node-selection/syntax.md), and treating the seed like a model.

For example, the following would run all models downstream of a seed named `country_codes`:

```shell
$ dbt run --select country_codes+
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
