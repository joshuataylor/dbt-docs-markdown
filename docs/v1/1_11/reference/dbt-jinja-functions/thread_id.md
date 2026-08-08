# About thread\_id

The `thread_id` outputs an identifier for the current Python thread that is executing a node, like `Thread-1`.

This value is useful when auditing or analyzing dbt invocation metadata. It corresponds to the `thread_id` within the [`Result` object](../dbt-classes.md#result-objects) and [`run_results.json`](../artifacts/run-results-json.md).

If available, the `thread_id` is:

* available in the compilation context of [`query-comment`](../project-configs/query-comment.md)
* included in the `info` dictionary in dbt [events and logs](../events-logging.md#info)
* included in the `metadata` dictionary in [dbt artifacts](../artifacts/dbt-artifacts.md#common-metadata)
* included as a label in all BigQuery jobs that dbt originates
