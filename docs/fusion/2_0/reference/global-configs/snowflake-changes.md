# Snowflake adapter behavior changes

## The `snowflake_default_transient_dynamic_tables` flag[​](#the-snowflake_default_transient_dynamic_tables-flag "Direct link to the-snowflake_default_transient_dynamic_tables-flag")

Available starting `dbt-snowflake` v1.12. The `snowflake_default_transient_dynamic_tables` flag controls whether Snowflake dynamic tables are created as transient when the model config does not explicitly set the [`transient`](https://docs.getdbt.com/reference/resource-configs/snowflake-configs.md#transient-dynamic-tables) config.

* When set to `false` (default): Dynamic tables are created as permanent tables with a [Fail-safe period](https://docs.snowflake.com/en/user-guide/data-failsafe) unless you set `transient: true` for a specific model.
* When set to `true`: Dynamic tables are created as transient (no Fail-safe period) when `transient` is not specified in the model config. Transient dynamic tables can reduce storage costs.

Set the `snowflake_default_transient_dynamic_tables` flag in your `dbt_project.yml` under the `flags` key. You can override the default setting using the [`transient`](https://docs.getdbt.com/reference/resource-configs/snowflake-configs.md#transient-dynamic-tables) config on dynamic table models.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
