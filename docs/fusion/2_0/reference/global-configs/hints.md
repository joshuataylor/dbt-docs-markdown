# CLI hints

💡Did you know\...

Available from dbt v

<!-- -->

1.12

<!-- -->

or with the

<!-- -->

[dbt "Latest" release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md).

dbt surfaces occasional, non-blocking hints during command execution that point toward ways to reduce build and parse times. Hints appear as `[HINT]`-prefixed notes in the CLI output; they are informational only and never block a run.

Each hint is shown, at most, once per week per project target directory.

## Disabling hints[​](#disabling-hints "Direct link to Disabling hints")

Hints are enabled by default. To disable them, use any of the following:

* **CLI**: `--no-hints-enabled`

* **Environment variable**: `DBT_ENGINE_HINTS_ENABLED=false`

* **Project file**: Set `hints_enabled: false` in `dbt_project.yml`:

  dbt\_project.yml

  ```yaml
  flags:
    hints_enabled: false
  ```

## Available hints[​](#available-hints "Direct link to Available hints")

The following hints are available starting dbt Core v1.12.

### Large build without state or deferral[​](#large-build-without-state-or-deferral "Direct link to Large build without state or deferral")

Appears during `dbt build` when more than 100 models are selected and state or deferral (`--state`, `--defer`, `--defer-state`) is not in use. Suggests using [dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md), [deferral](https://docs.getdbt.com/reference/node-selection/defer.md), or [`dbt clone`](https://docs.getdbt.com/reference/commands/clone.md) to skip unnecessary work and reuse existing results, or [node selectors](https://docs.getdbt.com/reference/node-selection/syntax.md) to reduce the number of models selected.

### Long parsing without v2 parser[​](#long-parsing-without-v2-parser "Direct link to Long parsing without v2 parser")

Appears after manifest load when parsing takes 30 seconds or longer and the [Opt-in v2 parser](https://docs.getdbt.com/reference/global-configs/parsing.md#opt-in-v2-parser) is not enabled. Suggests enabling `--use-v2-parser` to reduce parse time.
