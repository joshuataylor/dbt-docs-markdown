Compute controls whether a unit test runs against your data platform or locally with DuckDB.

# compute [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Available in v2

The `compute` config is available in v2 only. It isn't available in v1 and will be ignored. To upgrade to v2, refer to [Upgrade to v2](../../docs/dbt-versions/dbt-upgrade/upgrading-to-v2.md) upgrade guide.

By default, each unit test sends a query to your data platform and waits for the result. This can slow down testing and use warehouse compute.

Use the `compute: local` config to run [unit tests](../../docs/build/unit-tests.md) locally with DuckDB for faster feedback while you develop while managing warehouse compute cost.

models/filename.yml

```yml
unit_tests:
  - name: unit_test_name
    model: model_name
    config:
      compute: local | remote
```

dbt\_project.yml

```yml
unit_tests:
  resource-path:
    +compute: local | remote
```

## Definition

You can configure `compute` for [unit tests](../../docs/build/unit-tests.md), either on an individual test in a properties YAML file or for a group of tests in your project YAML file (`dbt_project.yml`).

The following values are available for `compute`:

* `local`: Run the unit test with DuckDB, wherever dbt is running. Nothing is sent to your data platform, so the test returns quickly and uses no warehouse compute.
* `remote` (default): Send the unit test to your data platform to run, using warehouse compute like any other query. Because this is the default, you only need to set it explicitly to opt a test out of a project-level `+compute: local`.

Local execution is available for Snowflake and BigQuery. You might also see `sidecar` in error messages, which means the same thing as `local`.

A config on an individual unit test overrides the project-level config. So if you set `+compute: local` in your `dbt_project.yml` and `compute: remote` in one test, that single test runs against your data platform while the rest run locally.

In your project file:

dbt\_project.yml

```yml
unit_tests:
  my_project:
    +compute: local
```

Then on a single test:

models/filename.yml

```yml
unit_tests:
  - name: test_revenue_calculation
    model: fct_revenue
    config:
      compute: remote
```

#### Things to know about local execution

* Your SQL has to be translatable to DuckDB. Platform-specific functions with no DuckDB equivalent fail, and `local` doesn't fall back to your data platform — a translation failure is a test failure.
* To translate your SQL, dbt fetches the schemas of your model's direct upstream models from your data platform the first time you run the test, then caches them for later runs. Those upstream models must already exist in your data platform.
* Setting `compute: local` also promotes [`static_analysis`](./static-analysis.md) to `strict` for that test, because local execution needs strict analysis to translate your SQL. If you set `static_analysis: off` on the test, it can't run locally and produces an error.

## Related docs

* [Run unit tests locally](../../docs/build/unit-tests.md#run-unit-tests-locally)
* [Unit tests](../../docs/build/unit-tests.md)
* [About unit tests property](../resource-properties/unit-tests.md)
* [static\_analysis](./static-analysis.md)
