*
* [Commands](../reference/dbt-commands.md)
* [Flags (global configs)](../reference/global-configs/about-global-configs.md)
* Available flags

# Available flags

The list of flags available in dbt.

## [Anonymous usage stats](../reference/global-configs/usage-stats.md)

[dbt Labs is on a mission to build the best version of dbt possible, and a crucial part of that is understanding how users work with dbt. To this end, we've added some simple event tracking (or telemetry) to dbt using Snowplow. Importantly, we do not track credentials, raw model contents, or model names: we consider these private, and frankly none of our business.](../reference/global-configs/usage-stats.md)

## [Checking version compatibility](../reference/global-configs/version-compatibility.md)

[For the first several years of 's development, breaking changes were more common. For this reason, we encouraged setting dbt version requirements \&mdash; especially if they use features that are newer or which may break in future versions of . By default, if you run a project with an incompatible dbt version, dbt will raise an error.](../reference/global-configs/version-compatibility.md)

## [Logs](../reference/global-configs/logs.md)

[Log formatting](../reference/global-configs/logs.md)

## [Failing fast](../reference/global-configs/failing-fast.md)

[Supply the -x or --fail-fast flag to dbt run, dbt build, or dbt test to make dbt exit immediately on the first failure. fail\_fast is a global flag, so it stops on both run errors and test errors —\&mdash; not just run errors. If other models are in-progress when the first failure happens, then dbt will terminate the connections for these still-running models.](../reference/global-configs/failing-fast.md)

## [Indirect selection](../reference/global-configs/indirect-selection.md)

[Indirect selection determines which tests to run when you select models or other resources. It applies to tests that are related to your selected resources through relationships in your DAG \&mdash; for example, tests on upstream or downstream models, or tests that reference multiple models.](../reference/global-configs/indirect-selection.md)

## [JSON artifacts](../reference/global-configs/json-artifacts.md)

[Write JSON artifacts](../reference/global-configs/json-artifacts.md)

## [Parsing](../reference/global-configs/parsing.md)

[Partial Parsing](../reference/global-configs/parsing.md)

## [SQL parse](../reference/global-configs/sqlparse.md)

[Configure sqlparse grouping limits when dbt compiles SQL.](../reference/global-configs/sqlparse.md)

## [Print output](../reference/global-configs/print-output.md)

[Suppress print() messages in stdout](../reference/global-configs/print-output.md)

## [Record timing info](../reference/global-configs/record-timing-info.md)

[The -r or --record-timing-info flag saves performance profiling information to a file. This file can be visualized with snakeviz to understand the performance characteristics of a dbt invocation.](../reference/global-configs/record-timing-info.md)

## [Resource type](../reference/global-configs/resource-type.md)

[The --resource-type and --exclude-resource-type flags include or exclude resource types from the dbt build, dbt clone, dbt test, and dbt list commands.](../reference/global-configs/resource-type.md)

## [Static analysis](../reference/global-configs/static-analysis-flag.md)

[Use the --static-analysis flag to override model-level static\_analysis behavior for a single run.](../reference/global-configs/static-analysis-flag.md)

## [Warnings](../reference/global-configs/warnings.md)

[Use --warn-error to promote all warnings to errors](../reference/global-configs/warnings.md)

[Previous](../reference/global-configs/user-settings.md)

[User settings](../reference/global-configs/user-settings.md)

[Next](../reference/global-configs/usage-stats.md)

[Anonymous usage stats](../reference/global-configs/usage-stats.md)
