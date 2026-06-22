# Amazon Redshift adapter behavior changes

The following are the current [behavior change flags](https://docs.getdbt.com/reference/global-configs/behavior-changes.md#behavior-change-flags) that are specific to `dbt-redshift`:

| Flag                                                                                                       | `dbt-redshift`: Intro | `dbt-redshift`: Maturity | Status |
| ---------------------------------------------------------------------------------------------------------- | --------------------- | ------------------------ | ------ |
| [`redshift_skip_autocommit_transaction_statements`](#redshift_skip_autocommit_transaction_statements-flag) | 1.12.0                | TBD                      | Active |

## The `redshift_skip_autocommit_transaction_statements` flag[​](#redshift_skip_autocommit_transaction_statements-flag "Direct link to redshift_skip_autocommit_transaction_statements-flag")

Available starting `dbt-redshift` 1.12.0.

The `redshift_skip_autocommit_transaction_statements` flag controls whether dbt sends explicit [`BEGIN`](https://docs.aws.amazon.com/redshift/latest/dg/r_BEGIN.html), [`COMMIT`](https://docs.aws.amazon.com/redshift/latest/dg/r_COMMIT.html), and [`ROLLBACK`](https://docs.aws.amazon.com/redshift/latest/dg/r_ROLLBACK.html) statements to Amazon Redshift when autocommit is enabled. Since `dbt-redshift` 1.5, the default Redshift connection uses `autocommit=True`, so the driver already commits each statement. In that setup, the statements are redundant when autocommit is enabled and add extra round trips to Redshift.

* When set to `false` (default), dbt sends `BEGIN`, `COMMIT`, and `ROLLBACK` statements (legacy behavior).

* When set to `true`, dbt does not send `BEGIN`, `COMMIT`, or `ROLLBACK` statements to Redshift, which can reduce round trips and improve performance. dbt still calls `begin()`, `commit()`, and `rollback_if_open()` on the connection, but no longer issues the matching SQL:

  * `begin()` is invoked, but dbt does not execute `BEGIN` on Redshift.
  * `commit()` is invoked, but dbt does not execute `COMMIT` on Redshift.
  * `rollback_if_open()` is invoked, but dbt does not execute `ROLLBACK` on Redshift.

dbt still maintains its internal `transaction_open` state so transaction tracking remains consistent even when those SQL statements are skipped.

To skip unnecessary transaction statements when `autocommit` is enabled, set the flag to `true` in your `dbt_project.yml`:

dbt\_project.yml

```yaml
flags:
  redshift_skip_autocommit_transaction_statements: true
```

### How this flag interacts with `autocommit`[​](#how-this-flag-interacts-with-autocommit "Direct link to how-this-flag-interacts-with-autocommit")

* If the connection uses `autocommit=False`, dbt’s explicit transaction behavior is unchanged.
* If the connection uses `autocommit=True` (default) and the flag is `false` (default), dbt still sends `BEGIN`, `COMMIT`, and `ROLLBACK`.
* If the connection uses `autocommit=True` and the flag is `true`, dbt skips those statements.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
