# check\_cols

dbt\_project.yml

```yml
snapshots:
  <resource-path>:
    +strategy: check
    +check_cols: [column_name] | all
```

## Description[​](#description "Direct link to Description")

A list of columns within the results of your snapshot query to check for changes.

Alternatively, use all columns using the `all` value (however this may be less performant).

This parameter is **required if using the `check` [strategy](https://docs.getdbt.com/reference/resource-configs/strategy.md)**.

## Default[​](#default "Direct link to Default")

No default is provided.

## Examples[​](#examples "Direct link to Examples")

### Check a list of columns for changes[​](#check-a-list-of-columns-for-changes "Direct link to Check a list of columns for changes")

<!-- -->

### Check all columns for changes[​](#check-all-columns-for-changes "Direct link to Check all columns for changes")

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
