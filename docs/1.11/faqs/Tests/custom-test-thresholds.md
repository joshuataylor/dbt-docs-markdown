# Can I set test failure thresholds?

You can use the `error_if` and `warn_if` configs to set custom failure thresholds in your tests. For more details, see [reference](https://docs.getdbt.com/reference/resource-configs/severity.md) for more information.

You can also try the following solutions:

* Setting the [severity](https://docs.getdbt.com/reference/resource-configs/severity.md) to `warn` or `error`
* Writing a [custom generic test](https://docs.getdbt.com/best-practices/writing-custom-generic-tests.md) that accepts a threshold argument ([example](https://discourse.getdbt.com/t/creating-an-error-threshold-for-schema-tests/966))

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
