# strategy

* timestamp
* check

dbt\_project.yml

```yml
snapshots:
  <resource-path>:
    +strategy: timestamp
    +updated_at: column_name
```

dbt\_project.yml

```yml
snapshots:
  <resource-path>:
    +strategy: check
    +check_cols: [column_name] | all
```

## Description[​](#description "Direct link to Description")

The snapshot strategy dbt should use to detect record changes. Read the guide to [snapshots](https://docs.getdbt.com/docs/build/snapshots.md#detecting-row-changes) to understand the differences between the two.

## Default[​](#default "Direct link to Default")

This is a **required configuration**. There is no default value.

## Examples[​](#examples "Direct link to Examples")

### Use the timestamp strategy[​](#use-the-timestamp-strategy "Direct link to Use the timestamp strategy")

<!-- -->

### Use the check strategy[​](#use-the-check-strategy "Direct link to Use the check strategy")

<!-- -->

### Advanced: define and use custom snapshot strategy[​](#advanced-define-and-use-custom-snapshot-strategy "Direct link to Advanced: define and use custom snapshot strategy")

Behind the scenes, snapshot strategies are implemented as macros, named `snapshot_<strategy>_strategy`

* [Source code](https://github.com/dbt-labs/dbt-adapters/blob/60005a0a2bd33b61cb65a591bc1604b1b3fd25d5/dbt/include/global_project/macros/materializations/snapshots/strategies.sql#L52) for the timestamp strategy
* [Source code](https://github.com/dbt-labs/dbt-adapters/blob/60005a0a2bd33b61cb65a591bc1604b1b3fd25d5/dbt/include/global_project/macros/materializations/snapshots/strategies.sql#L136) for the check strategy

It's possible to implement your own snapshot strategy by adding a macro with the same naming pattern to your project. For example, you might choose to create a strategy which records hard deletes, named `timestamp_with_deletes`.

1. Create a macro named `snapshot_timestamp_with_deletes_strategy`. Use the existing code as a guide and adjust as needed.
2. Use this strategy via the `strategy` configuration:

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
