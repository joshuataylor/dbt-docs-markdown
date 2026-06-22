# deprecation\_date

models/\<schema>.yml

```yml
models:
  - name: my_model
    description: deprecated
    deprecation_date: 1999-01-01 00:00:00.00+00:00
```

models/\<schema>.yml

```yml
models:
  - name: my_model
    description: deprecating in the future
    deprecation_date: 2999-01-01 00:00:00.00+00:00
```

## Definition[​](#definition "Direct link to Definition")

The deprecation date of the model is formatted as a date, optionally with a timezone offset. Supported RFC 3339 formats include:

* `YYYY-MM-DD hh:mm:ss.sss±hh:mm`
* `YYYY-MM-DD hh:mm:ss.sss`
* `YYYY-MM-DD`

When `deprecation_date` does not include an offset from UTC, then it is interpreted as being in the system time zone of the dbt execution environment.

## Explanation[​](#explanation "Direct link to Explanation")

### Purpose[​](#purpose "Direct link to Purpose")

Declaring a `deprecation_date` for a dbt model provides a mechanism to communicate plans and timelines for long-term support and maintenance and to facilitate change management.

Setting a `deprecation_date` works well in conjunction with other [model governance](https://docs.getdbt.com/docs/mesh/govern/about-model-governance.md) features like [model versions](https://docs.getdbt.com/docs/mesh/govern/model-versions.md), but can also be used independently from them.

info

If your model has an [enforced contract](https://docs.getdbt.com/docs/mesh/govern/model-contracts.md), you cannot delete the model until after the `deprecation_date` has passed. dbt doesn't allow deleting models with enforced contracts before their `deprecation_date` to protect downstream consumers.

If you try to delete a versioned model before its `deprecation_date`, dbt will raise an error during development runs and cause jobs to fail.

### Warning messages[​](#warning-messages "Direct link to Warning messages")

When a project references a model that's slated for deprecation or the deprecation date has passed, a warning is generated. If it's a versioned model, with a newer version available, then the warning says so. This added bit of cross-team communication, from producers to consumers, is an advantage of using dbt's built-in functionality around model versions to facilitate migrations.

Additionally, [`WARN_ERROR_OPTIONS`](https://docs.getdbt.com/reference/global-configs/warnings.md) gives a mechanism whereby users can promote these warnings to actual runtime errors:

| Warning                        | Scenario                                           | Affected projects      |
| ------------------------------ | -------------------------------------------------- | ---------------------- |
| `DeprecatedModel`              | Parsing a project that defines a deprecated model  | Producer               |
| `DeprecatedReference`          | Referencing a model with a past deprecation date   | Producer and consumers |
| `UpcomingReferenceDeprecation` | Referencing a model with a future deprecation date | Producer and consumers |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

**Example**

Example output for an `UpcomingReferenceDeprecation` warning:

```text
$ dbt parse
15:48:14  Running with dbt=1.6.0
15:48:14  Registered adapter: postgres=1.6.0
15:48:14  [WARNING]: While compiling 'my_model_ref': Found a reference to my_model, which is slated for deprecation on '2038-01-19T03:14:07-00:00'.
```

### Selection syntax[​](#selection-syntax "Direct link to Selection syntax")

There is not specific [node selection syntax](https://docs.getdbt.com/reference/node-selection/syntax.md) for `deprecation_date`. [Programmatic invocations](https://docs.getdbt.com/reference/programmatic-invocations.md) is one way to identify deprecated models (potentially in conjunction with [dbt list](https://docs.getdbt.com/reference/commands/list.md)). e.g., `dbt ls -q --output json --output-keys database schema alias deprecation_date`.

### Deprecation process[​](#deprecation-process "Direct link to Deprecation process")

Additional steps are necessary to save on build-related compute and storage costs for a deprecated model.

Deprecated models can continue to be built by producers and be selected by consumers until they are [disabled](https://docs.getdbt.com/reference/resource-configs/enabled.md) or removed.

Just like it does not automatically [drop relations when models are deleted](https://docs.getdbt.com/faqs/Models/removing-deleted-models.md), dbt does not drop relations for deprecated models.

Strategies similar to [here](https://discourse.getdbt.com/t/faq-cleaning-up-removed-models-from-your-production-schema/113) or [here](https://discourse.getdbt.com/t/clean-your-warehouse-of-old-and-deprecated-models/1547) can be used to drop relations that have been deprecated and are no longer in use.

### Table expiration on BigQuery[​](#table-expiration-on-bigquery "Direct link to Table expiration on BigQuery")

dbt-bigquery can set an [`hours_to_expiration`](https://docs.getdbt.com/reference/resource-configs/bigquery-configs.md#controlling-table-expiration) that translates to `expiration_timestamp` within BigQuery.

dbt does not automatically synchronize `deprecation_date` and `hours_to_expiration`, but users may want to coordinate them in some fashion (such as setting a model to expire 48 hours after its `deprecation_date`). Expired tables in BigQuery will be deleted and their storage reclaimed.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
