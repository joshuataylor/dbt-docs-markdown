# BigQuery adapter behavior changes

## The `bigquery_use_batch_source_freshness` flag[​](#the-bigquery_use_batch_source_freshness-flag "Direct link to the-bigquery_use_batch_source_freshness-flag")

The `bigquery_use_batch_source_freshness` flag is `false` by default. Setting it to `true` in your `dbt_project.yml` file enables dbt to compute `source freshness` results with a single batched query to BigQuery's [`INFORMATION_SCHEMA.TABLE_STORAGE`](https://cloud.google.com/bigquery/docs/information-schema-table-storage) view as opposed to sending a metadata request for each source.

Setting this flag to `true` improves the performance of the `source freshness` command significantly, especially when a project contains a large (1000+) number of sources.

Using `loaded_at_field` with batch freshness

With `bigquery_use_batch_source_freshness` enabled, dbt determines freshness from BigQuery metadata tables.

When you configure a `loaded_at_field` on a source, dbt runs a column-based freshness check for that source instead of metadata-based freshness.

However, if `loaded_at_field` is set on *all* sources, freshness fails with a compilation error (`list object has no element 0` in `get_relation_last_modified`). This occurs because the `bigquery_use_batch_source_freshness` flag assumes at least one source uses metadata-based freshness; configuring `loaded_at_field` on all sources breaks this assumption.

To avoid this, remove `loaded_at_field` from any sources you want checked using batch freshness.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
