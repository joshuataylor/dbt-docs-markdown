# About dbt --version

The `--version` command-line flag returns information about the currently installed version of dbt v1, the dbt platform CLI, or dbt v2. This flag is not supported when invoking dbt in other dbt runtimes (for example, the IDE or scheduled runs).

* **dbt v1** — Returns the installed version of dbt v1 and the versions of all installed adapters.
* **dbt platform CLI** — Returns the installed version of the [dbt platform CLI](../../docs/platform/dbt-cli-installation.md) and, for the other `dbt_version` values, the *latest* version of the dbt runtime in dbt.
* **dbt v2** — Returns the installed dbt v2 version. Because the CLI and language server ship in a single binary, they always report the same version. Refer to [Version compatibility](../../docs/dbt-versions/dbt-version-compatibility.md) for how this maps to the dbt VS Code extension.

## Versioning

To learn more about release versioning for dbt v1, refer to [How dbt v1 uses semantic versioning](../../docs/dbt-versions.md#how-dbt-v1-uses-semantic-versioning).

If using a [dbt release track](../../docs/dbt-versions/dbt-release-tracks.md), which provide ongoing updates to dbt, then `dbt_version` represents the release version of dbt in dbt. This also follows semantic versioning guidelines, using the `YYYY.M.D+<suffix>` format. The year, month, and day represent the date the version was built (for example, `2024.10.8+996c6a8`). The suffix provides an additional unique identification for each build.

## Example usages

dbt v1 example:

dbt v1

```text
$ dbt --version
Core:
  - installed: 1.7.6
  - latest:    1.7.6 - Up to date!
Plugins:
  - snowflake: 1.7.1 - Up to date!
```

dbt CLI example:

dbt platform CLI

```text
$ dbt --version
Cloud CLI - 0.35.7 (fae78a6f5f6f2d7dff3cab3305fe7f99bd2a36f3 2024-01-18T22:34:52Z)
```

dbt v2 example:

```shell
$ dbt --version
dbt-fusion 2.0.0-preview.92
```

For a machine-readable version, add the `--format json` flag. This is useful when filing a bug report or when tooling needs to parse the installed version:

```shell
dbt --version --format json
```

```json
{
  "version": "2.0.0-preview.92"
}
```
