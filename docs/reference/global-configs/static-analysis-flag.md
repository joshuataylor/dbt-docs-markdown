# Static analysis

Use the `--static-analysis` flag to override model-level `static_analysis` behavior for a single run. This flag applies to the dbt Fusion engine only; it is ignored by dbt Core.

Values:

* `baseline` (default): Statically analyze SQL for all models in the run. This is the recommended starting point for users transitioning from dbt Core.
* `strict` (previously `on`): Statically analyze all SQL before execution begins. Provides maximum validation guarantees — nothing runs until the entire project is proven valid.
* `off`: Disable static analysis for all models in the run.

Deprecated values

The `on` and `unsafe` values are deprecated and will be removed in May 2026. Use `strict` instead.

If not set, Fusion defaults to `baseline` mode, which provides a smooth transition from dbt Core while still catching most SQL errors. See [Configuring `static_analysis`](https://docs.getdbt.com/docs/fusion/new-concepts.md#configuring-static_analysis) for more information on incrementally opting in to stricter analysis.

Usage

```shell
dbt run --static-analysis strict
dbt run --static-analysis baseline
dbt run --static-analysis off
```

## Related docs[​](#related-docs "Direct link to Related docs")

Also check out the model-level [`static_analysis` (resource config)](https://docs.getdbt.com/reference/resource-configs/static-analysis.md) and [About flags](https://docs.getdbt.com/reference/global-configs/about-global-configs.md) pages for more details.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
