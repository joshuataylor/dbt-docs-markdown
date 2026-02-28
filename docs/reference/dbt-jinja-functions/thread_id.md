# About thread\_id

The `thread_id` outputs an identifier for the current Python thread that is executing a node, like `Thread-1`.

This value is useful when auditing or analyzing dbt invocation metadata. It corresponds to the `thread_id` within the [`Result` object](https://docs.getdbt.com/reference/dbt-classes.md#result-objects) and [`run_results.json`](https://docs.getdbt.com/reference/artifacts/run-results-json.md).

If available, the `thread_id` is:

* available in the compilation context of [`query-comment`](https://docs.getdbt.com/reference/project-configs/query-comment.md)
* included in the `info` dictionary in dbt [events and logs](https://docs.getdbt.com/reference/events-logging.md#info)
* included in the `metadata` dictionary in [dbt artifacts](https://docs.getdbt.com/reference/artifacts/dbt-artifacts.md#common-metadata)
* included as a label in all BigQuery jobs that dbt originates

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
