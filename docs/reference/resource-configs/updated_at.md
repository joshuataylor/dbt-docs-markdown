# updated\_at

dbt\_project.yml

```yml
snapshots:
  <resource-path>:
    +strategy: timestamp
    +updated_at: column_name
```

<!-- -->

## Description[​](#description "Direct link to Description")

A column within the results of your snapshot query that represents when the record row was last updated.

This parameter is **required if using the `timestamp` [strategy](https://docs.getdbt.com/reference/resource-configs/strategy.md)**. The `updated_at` field may support ISO date strings and unix epoch integers, depending on the data platform you use.

## Default[​](#default "Direct link to Default")

No default is provided.

## Examples[​](#examples "Direct link to Examples")

### Use a column name `updated_at`[​](#use-a-column-name-updated_at "Direct link to use-a-column-name-updated_at")

<!-- -->

### Coalesce two columns to create a reliable `updated_at` column[​](#coalesce-two-columns-to-create-a-reliable-updated_at-column "Direct link to coalesce-two-columns-to-create-a-reliable-updated_at-column")

Consider a data source that only has an `updated_at` column filled in when a record is updated (so a `null` value indicates that the record hasn't been updated after it was created).

Since the `updated_at` configuration only takes a column name, rather than an expression, you should update your snapshot query to include the coalesced column.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
