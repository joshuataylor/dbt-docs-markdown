# Sources JSON file

**Current schema:** [`v3`](https://schemas.getdbt.com/dbt/sources/v3/index.html)

**Produced by:** [`source freshness`](https://docs.getdbt.com/reference/commands/source.md)

This file contains information about [sources with freshness checks](https://docs.getdbt.com/docs/build/sources.md#checking-source-freshness). Today, dbt uses this file to power its [Source Freshness visualization](https://docs.getdbt.com/docs/build/sources.md#source-data-freshness).

### Top-level keys[​](#top-level-keys "Direct link to Top-level keys")

* [`metadata`](https://docs.getdbt.com/reference/artifacts/dbt-artifacts.md#common-metadata)
* `elapsed_time`: Total invocation time in seconds.
* `results`: Array of freshness-check execution details.

Each entry in `results` is a dictionary with the following keys:

* `unique_id`: Unique source node identifier, which map results to `sources` in the [manifest](https://docs.getdbt.com/reference/artifacts/manifest-json.md)
* `max_loaded_at`: Max value of `loaded_at_field` timestamp in the source table when queried.
* `snapshotted_at`: Current timestamp when querying.
* `max_loaded_at_time_ago_in_s`: Interval between `max_loaded_at` and `snapshotted_at`, calculated in python to handle timezone complexity.
* `criteria`: The freshness threshold(s) for this source, defined in the project.
* `status`: The freshness status of this source, based on `max_loaded_at_time_ago_in_s` + `criteria`, reported on the CLI. One of `pass`, `warn`, or `error` if the query succeeds, `runtime error` if the query fails.
* `execution_time`: Total time spent checking freshness for this source
* `timing`: Array that breaks down execution time into steps (`compile` + `execute`)

<!-- -->

* `adapter_response`: Dictionary of metadata returned from the database, which varies by adapter. For example, success `code`, number of `rows_affected`, total `bytes_processed`, and so on. Not applicable for [data tests](https://docs.getdbt.com/docs/build/data-tests.md).
  <!-- -->
  * `rows_affected` returns the number of rows modified by the last statement executed. In cases where the query's row count can't be determined or isn't applicable (such as when creating a view), a [standard value](https://peps.python.org/pep-0249/#rowcount) of `-1` is returned for `rowcount`.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
