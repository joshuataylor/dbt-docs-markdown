# About state in dbt

One of the greatest underlying assumptions about dbt is that its operations should be **stateless** and **idempotent**. That is, it doesn't matter how many times a model has been run before, or if it has ever been run before. It doesn't matter if you run it once or a thousand times. Given the same raw data, you can expect the same transformed result. A given run of dbt doesn't need to "know" about *any other* run; it just needs to know about the code in the project and the objects in your database as they exist *right now*.

That said, dbt does store "state" — a detailed, point-in-time view of project resources (also referred to as nodes), database objects, and invocation results — in the form of its [artifacts](https://docs.getdbt.com/docs/deploy/artifacts.md). If you choose, dbt can use these artifacts to inform certain operations. Crucially, the operations themselves are still stateless and idempotent: given the same manifest and the same raw data, dbt will produce the same transformed result.

dbt can leverage artifacts from a prior invocation as long as their file path is passed to the `--state` flag. This is a prerequisite for:

* [The `state` selector](https://docs.getdbt.com/reference/node-selection/methods.md#state), whereby dbt can identify resources that are new or modified by comparing code in the current project against the state manifest.
* [Deferring](https://docs.getdbt.com/reference/node-selection/defer.md) to another environment, whereby dbt can identify upstream, unselected resources that don't exist in your current environment and instead "defer" their references to the environment provided by the state manifest.
* The [`dbt clone` command](https://docs.getdbt.com/reference/commands/clone.md), whereby dbt can clone nodes based on their location in the manifest provided to the `--state` flag.

Together, the [`state`](https://docs.getdbt.com/reference/node-selection/methods.md#state) selector and deferral enable ["slim CI"](https://docs.getdbt.com/best-practices/best-practice-workflows.md#run-only-modified-models-to-test-changes-slim-ci). We expect to add more features in future releases that can leverage artifacts passed to the `--state` flag.

## Related docs[​](#related-docs "Direct link to Related docs")

* [Configure state selection](https://docs.getdbt.com/reference/node-selection/configure-state.md)
* [State comparison caveats](https://docs.getdbt.com/reference/node-selection/state-comparison-caveats.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
