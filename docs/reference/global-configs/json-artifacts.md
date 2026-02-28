# JSON artifacts

### Write JSON artifacts[​](#write-json-artifacts "Direct link to Write JSON artifacts")

The `WRITE_JSON` config determines whether dbt writes [JSON artifacts](https://docs.getdbt.com/reference/artifacts/dbt-artifacts.md) (for example, `manifest.json`, `run_results.json`) to the `target/` directory. JSON serialization can be slow, and turning this flag off *might* make invocations of dbt faster. Alternatively, you can disable this config to perform a dbt operation and avoid overwriting artifacts from a previous run step.

Usage

```text
dbt run --no-write-json 
```

### Target path[​](#target-path "Direct link to Target path")

By default, dbt will write JSON artifacts and compiled SQL files to a directory named `target/`. This directory is located relative to `dbt_project.yml` of the active project.

Just like other global configs, it is possible to override these values for your environment or invocation by using the CLI option (`--target-path`) or environment variables (`DBT_TARGET_PATH`).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
