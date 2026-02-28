# BigQuery adapter behavior changes

## The `bigquery_use_batch_source_freshness` flag[​](#the-bigquery_use_batch_source_freshness-flag "Direct link to the-bigquery_use_batch_source_freshness-flag")

The `bigquery_use_batch_source_freshness` flag is `False` by default. Setting it to `True` in your `dbt_project.yml` file enables dbt to compute `source freshness` results with a single batched query to BigQuery's [`INFORMATION_SCHEMA.TABLE_STORAGE`](https://cloud.google.com/bigquery/docs/information-schema-table-storage) view as opposed to sending a metadata request for each source.

Setting this flag to `True` improves the performance of the `source freshness` command significantly, especially when a project contains a large (1000+) number of sources.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
