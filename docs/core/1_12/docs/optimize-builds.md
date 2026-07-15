# Optimize your dbt builds

By default, dbt rebuilds every selected node on every run — even if nothing has changed. This makes it easy to get started, but adds up for larger projects: longer job times, higher warehouse costs, and slower feedback during development. dbt has features designed to help you skip unnecessary work and reuse existing results instead.

## dbt State[​](#dbt-state "Direct link to dbt State")

[dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md) is a service that makes dbt smarter about what to build. It integrates into any dbt deployment, including the dbt platform and the self-hosted dbt Fusion engine and dbt Core.

Instead of rebuilding every node on every run, it compares each node's logic and upstream data against the previous run and picks the most efficient path:

* **Skip** — If the node's logic and upstream data haven't changed, dbt reuses the existing object in your target schema as-is.
* **Clone** — If the data is fresh but exists in a different schema (for example, production), dbt clones it rather than rebuilding from scratch.
* **Build** — If reuse isn't possible, dbt rebuilds normally and automatically defers unselected upstream nodes to production — no `--defer` or `--state` flags required.

To enable dbt State:

* **dbt Core v1.7–1.12**:

  ```bash
  cd path/to/your/project
  pip install dbt-state
  ```

* **dbt Core v2**:

  ```bash
  cd path/to/your/project
  dbt login
  ```

Authentication requires a dbt platform account on a Starter or Enterprise plan with a [free trial](https://docs.getdbt.com/docs/deploy/dbt-state-trial.md). For more information, refer to [Setting up dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-setup.md).

## Deferral[​](#deferral "Direct link to Deferral")

[Deferral](https://docs.getdbt.com/reference/node-selection/defer.md) lets you build a subset of your project without building all upstream dependencies first. Instead of running everything upstream, dbt points unbuilt references at existing objects in another environment — typically production.

This is useful in development and CI environments, where you want to test one or two models without waiting for a full pipeline run.

```bash
dbt build --select my_model --defer --state path/to/prod/artifacts
```

## dbt clone[​](#dbt-clone "Direct link to dbt clone")

[`dbt clone`](https://docs.getdbt.com/reference/commands/clone.md) creates copies of selected nodes in a target schema. On warehouses that support zero-copy cloning (for example, Snowflake), it creates lightweight database clones without duplicating the underlying data. On other warehouses, it creates views pointing at the upstream relations.

This is useful in development when you want to quickly populate a dev environment with production-like objects without running the full pipeline.

```bash
dbt clone --select my_model
```

## dbt's selection syntax[​](#dbts-selection-syntax "Direct link to dbt's selection syntax")

dbt has a [variety of selectors](https://docs.getdbt.com/reference/node-selection/syntax.md) you can use to target specific parts of your project instead of building everything every time. For example, `+my_model` selects `my_model` and all of its upstream dependencies, while `my_model+2` selects `my_model` and two levels of downstream dependents. This lets you test your changes in isolation without running your entire project.

```bash
dbt build --select +my_model
```
