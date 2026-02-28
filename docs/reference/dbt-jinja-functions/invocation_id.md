# About invocation\_id

The `invocation_id` outputs a UUID generated for this dbt command. This value is useful when auditing or analyzing dbt invocation metadata.

If available, the `invocation_id` is:

* available in the compilation context of [`query-comment`](https://docs.getdbt.com/reference/project-configs/query-comment.md)
* included in the `info` dictionary in dbt [events and logs](https://docs.getdbt.com/reference/events-logging.md#info)
* included in the `metadata` dictionary in [dbt artifacts](https://docs.getdbt.com/reference/artifacts/dbt-artifacts.md#common-metadata)
* included as a label in all BigQuery jobs that dbt originates

**Example usage**: You can use the following example code for all data platforms. Remember to replace `TABLE_NAME` with the actual name of your target table:

`select '{{ invocation_id }}' as test_id from TABLE_NAME`

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
