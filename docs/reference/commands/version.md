# About dbt --version

The `--version` command-line flag returns information about the currently installed version of dbt Core or the dbt CLI. This flag is not supported when invoking dbt in other dbt runtimes (for example, the IDE or scheduled runs).

* **dbt Core** — Returns the installed version of dbt Core and the versions of all installed adapters.
* **dbt CLI** — Returns the installed version of the [dbt CLI](https://docs.getdbt.com/docs/cloud/cloud-cli-installation.md) and, for the other `dbt_version` values, the *latest* version of the dbt runtime in dbt.

## Versioning[​](#versioning "Direct link to Versioning")

To learn more about release versioning for dbt Core, refer to [How dbt Core uses semantic versioning](https://docs.getdbt.com/docs/dbt-versions/core.md#how-dbt-core-uses-semantic-versioning).

If using a [dbt release track](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md), which provide ongoing updates to dbt, then `dbt_version` represents the release version of dbt in dbt. This also follows semantic versioning guidelines, using the `YYYY.M.D+<suffix>` format. The year, month, and day represent the date the version was built (for example, `2024.10.8+996c6a8`). The suffix provides an additional unique identification for each build.

## Example usages[​](#example-usages "Direct link to Example usages")

dbt Core example:

dbt Core

```text
$ dbt --version
Core:
  - installed: 1.7.6
  - latest:    1.7.6 - Up to date!
Plugins:
  - snowflake: 1.7.1 - Up to date!
```

dbt CLI example:

dbt CLI

```text
$ dbt --version
Cloud CLI - 0.35.7 (fae78a6f5f6f2d7dff3cab3305fe7f99bd2a36f3 2024-01-18T22:34:52Z)
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
