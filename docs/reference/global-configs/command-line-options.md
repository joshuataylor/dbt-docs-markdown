# Command line options

For consistency, command-line interface (CLI) flags should come right after the `dbt` prefix and its subcommands. This includes "global" flags (supported for all commands). For a list of all dbt CLI flags you can set, refer to [Available flags](https://docs.getdbt.com/reference/global-configs/about-global-configs.md#available-flags). When set, CLI flags override [environment variables](https://docs.getdbt.com/reference/global-configs/environment-variable-configs.md) and [project flags](https://docs.getdbt.com/reference/global-configs/project-flags.md).

Environment variables contain a `DBT_` prefix.

For example, instead of using:

```bash
dbt --no-populate-cache run
```

You should use:

```bash
dbt run --no-populate-cache
```

Historically, passing flags (such as "global flags") *before* the subcommand is a legacy functionality that dbt Labs can remove at any time. We do not support using the same flag before and after the subcommand.

## Using boolean and non-boolean flags[​](#using-boolean-and-non-boolean-flags "Direct link to Using boolean and non-boolean flags")

You can construct your commands with boolean flags to enable or disable or with non-boolean flags that use specific values, such as strings.

* Non-boolean config
* Boolean config

Use this non-boolean config structure:

* Replacing `<SUBCOMMAND>` with the command this config applies to.
* `<THIS-CONFIG>` with the config you are enabling or disabling, and
* `<SETTING>` with the new setting for the config.

CLI flags

```text

<SUBCOMMAND> --<THIS-CONFIG>=<SETTING> 
```

### Example[​](#example "Direct link to Example")

CLI flags

```text

dbt run --printer-width=80 
dbt test --indirect-selection=eager
```

To enable or disable boolean configs:

* Use `<SUBCOMMAND>` this config applies to.
* Followed by `--<THIS-CONFIG>` to turn it on, or `--no-<THIS-CONFIG>` to turn it off.
* Replace `<THIS-CONFIG>` with the config you are enabling or disabling

CLI flags

```text
dbt <SUBCOMMAND> --<THIS-CONFIG> 
dbt <SUBCOMMAND> --no-<THIS-CONFIG> 
```

### Example[​](#example-1 "Direct link to Example")

CLI flags

```text

dbt run --version-check
dbt run --no-version-check 
```

## Config precedence[​](#config-precedence "Direct link to Config precedence")

<!-- -->

There are multiple ways of setting flags, which depend on the use case:

* **[Project-level `flags` in `dbt_project.yml`](https://docs.getdbt.com/reference/global-configs/project-flags.md):** Define version-controlled defaults for everyone running this project. Also, opt in or opt out of [behavior changes](https://docs.getdbt.com/reference/global-configs/behavior-changes.md) to manage your migration off legacy functionality.
* **[Environment variables](https://docs.getdbt.com/reference/global-configs/environment-variable-configs.md):** Define different behavior in different runtime environments (development vs. production vs. [continuous integration](https://docs.getdbt.com/docs/deploy/continuous-integration.md), or different behavior for different users in development (based on personal preferences).
* **[CLI options](https://docs.getdbt.com/reference/global-configs/command-line-options.md):** Define behavior specific to *this invocation*. Supported for all dbt commands.

The most specific setting "wins." If you set the same flag in all three places, the CLI option will take precedence, followed by the environment variable, and finally, the value in `dbt_project.yml`. If you set the flag in none of those places, it will use the default value defined within dbt.

Most flags can be set in all three places:

```yaml
# dbt_project.yml
flags:
  # set default for running this project -- anywhere, anytime, by anyone
  fail_fast: true
```

```bash
# set this environment variable to 'True' (bash syntax)
export DBT_FAIL_FAST=1
dbt run
```

```bash
dbt run --fail-fast # set to True for this specific invocation
dbt run --no-fail-fast # set to False
```

There are two categories of exceptions:

1. **Flags setting file paths:** Flags for file paths that are relevant to runtime execution (for example, `--log-path` or `--state`) cannot be set in `dbt_project.yml`. To override defaults, pass CLI options or set environment variables (`DBT_LOG_PATH`, `DBT_STATE`). Flags that tell dbt where to find project resources (for example, `model-paths`) are set in `dbt_project.yml`, but as a top-level key, outside the `flags` dictionary; these configs are expected to be fully static and never vary based on the command or execution environment.
2. **Opt-in flags:** Flags opting in or out of [behavior changes](https://docs.getdbt.com/reference/global-configs/behavior-changes.md) can *only* be defined in `dbt_project.yml`. These are intended to be set in version control and migrated via pull/merge request. Their values should not diverge indefinitely across invocations, environments, or users.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
