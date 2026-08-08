# About local state in dbt

Looking for a managed state experience?

If you want a managed experience for state with dbt to skip rerunning models that haven't changed, check out [dbt State](../../docs/deploy/dbt-state-about.md).

Why idempotence matters here

State selection and deferral rely on your models being [idempotent](../../best-practices/idempotence.md), meaning re-running with the same inputs returns the same result. For more info, refer to [Idempotence in dbt](../../best-practices/idempotence.md).

One of the greatest underlying assumptions about dbt is that its operations should be **stateless** and **idempotent**. That is, it doesn't matter how many times a model has been run before, or if it has ever been run before. It doesn't matter if you run it once or a thousand times. Given the same raw data, you can expect the same transformed result. A given run of dbt doesn't need to "know" about *any other* run; it just needs to know about the code in the project and the objects in your database as they exist *right now*.

That said, dbt does store "state" — a detailed, point-in-time view of project resources (also referred to as nodes), database objects, and invocation results — in the form of its [artifacts](../../docs/deploy/artifacts.md). If you choose, dbt can use these artifacts to inform certain operations. Crucially, the operations themselves are still stateless and idempotent: given the same manifest and the same raw data, dbt will produce the same transformed result.

dbt can leverage artifacts from a prior invocation as long as their file path is passed to the `--state` flag. This is a prerequisite for:

* [The `state` selector](./methods.md#state), whereby dbt can identify resources that are new or modified by comparing code in the current project against the state manifest.
* [Deferring](./defer.md) to another environment, whereby dbt can identify upstream, unselected resources that don't exist in your current environment and instead "defer" their references to the environment provided by the state manifest.
* The [`dbt clone` command](../commands/clone.md), whereby dbt can clone nodes based on their location in the manifest provided to the `--state` flag.

Together, the [`state`](./methods.md#state) selector and deferral enable ["slim CI"](../../best-practices/best-practice-workflows.md#run-only-modified-models-to-test-changes-slim-ci). We expect to add more features in future releases that can leverage artifacts passed to the `--state` flag.

## Related docs[​](#related-docs "Direct link to Related docs")

* [Configure state selection](./configure-state.md)
* [State comparison caveats](./state-comparison-caveats.md)
