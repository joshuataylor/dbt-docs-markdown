# The agent returns no metrics or empty results

* Confirm the project you authorized has a configured [Semantic Layer](../../../docs/use-dbt-semantic-layer/dbt-sl.md) with metrics and dimensions.
* Check that the user who connected has at least read-only access to that project — the agent only sees what the user can see.
* If you scoped the OAuth connection to a single project, make sure it's the project that contains your metrics.
