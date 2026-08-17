# Optimize static analysis for development and deployment

important

The dbt Fusion engine is currently available for installation in:

* [Local command line interface (CLI) tools](../docs/local/install-dbt.md?version=2) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [VS Code and Cursor with the dbt extension](../docs/install-dbt-extension.md) [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* [dbt platform environments](../docs/dbt-versions/upgrade-dbt-platform-version.md#dbt-fusion-engine)

Join the conversation in our Community Slack channel [`#dbt-fusion-engine`](https://getdbt.slack.com/archives/C088YCAB6GH).

Read the [Fusion Diaries](https://github.com/dbt-labs/dbt-core/discussions/categories/announcements?discussions_q=is:open+diaries+category:Announcements) for the latest updates.

Static analysis helps the dbt Fusion engine validate your SQL before it runs. This guide shows how to configure it so you get stronger checks while you develop, and faster, less blocking runs in deployment.

This guide explains why using `strict` in development and `baseline` (the default, lighter static analysis mode) in deployment is a valid and recommended pattern, and how to configure it in your Fusion project. For more information about modes and features, refer to [About static analysis](../docs/build/about-static-analysis.md).

## Why this pattern

* **Development:** `strict` mode has the strongest SQL checks before you promote changes, including richer column-level features in the VS Code extension.
* **Deployment:** `baseline` skips remote warehouse schema downloads and surfaces findings as warnings, so jobs are less likely to block. That can save compile time (and warehouse cost) in deployment, especially in projects with many sources. Review deployment logs for warnings that `strict` would have raised as errors in development.

`strict` can increase compile time because the dbt Fusion engine downloads schemas for all sources (including sources your models do not reference). Teams with thousands of sources have seen large differences between `baseline` and `strict`.

## Set the mode with the CLI flag

Use the [`--static-analysis`](../reference/global-configs/static-analysis-flag.md) flag to set the mode for a single run.

**Development:**

```bash
dbt compile --static-analysis strict
```

**Deployment:**

```bash
dbt compile --static-analysis baseline
```

You can use the same flag with `dbt run` or `dbt build`. If you already have dbt Core or the platform CLI installed alongside Fusion, use `dbtf` as the unambiguous Fusion command.

You can also configure [`static_analysis`](../reference/resource-configs/static-analysis.md) per directory or model. Refer to [Configuring `static_analysis`](../docs/build/about-static-analysis.md#configuring-static_analysis) for examples.

## Set the mode with an environment variable

You can also drive [`static_analysis`](../reference/resource-configs/static-analysis.md) from a custom environment variable in `dbt_project.yml`. This is useful when development and deployment share the same project config but set different environment values.

dbt\_project.yml

```yml
models:
  my_project:
    +static_analysis: "{{ env_var('DBT_ENV_STATIC_ANALYSIS', 'baseline') }}"
```

Then set the variable per environment:

* Development: `DBT_ENV_STATIC_ANALYSIS=strict`
* Deployment: leave unset (defaults to `baseline`), or set `DBT_ENV_STATIC_ANALYSIS=baseline`

`DBT_ENV_STATIC_ANALYSIS` is a custom variable name you choose. It is separate from the built-in `DBT_STATIC_ANALYSIS` override used with the CLI flag.

For more information about `env_var`, refer to [About env\_var](../reference/dbt-jinja-functions/env_var.md) and [Environment variables](../docs/build/environment-variables.md).

## Related docs

* [About static analysis](../docs/build/about-static-analysis.md)
* [`static_analysis` config](../reference/resource-configs/static-analysis.md)
* [`--static-analysis` flag](../reference/global-configs/static-analysis-flag.md)
