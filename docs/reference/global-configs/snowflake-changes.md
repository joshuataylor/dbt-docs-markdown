# Snowflake adapter behavior changes

## The `enable_truthy_nulls_equals_macro` flag[​](#the-enable_truthy_nulls_equals_macro-flag "Direct link to the-enable_truthy_nulls_equals_macro-flag")

The `enable_truthy_nulls_equals_macro` flag is `False` by default. Setting it to `True` in your `dbt_project.yml` file enables null-safe equality on the dbt equals macro, which is used in the incremental and snapshot materializations.

For example, when you compare NULL using `=` without the flag, it doesn't return `TRUE`, even when comparing `NULL = NULL`. Making it null safe allows for proper comparisons with `NULL`. If both values are `NULL`, it evaluates to `TRUE` instead of `UNKNOWN`.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
