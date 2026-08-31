# Deprecation & warnings overview

When using dbt, you may see warnings or other changes that need your attention. These changes help us move forward with the latest version of dbt and improve the experience for all users.

Use this page to understand the different types of changes, what to do, and where to find more information.

[![](/img/icons/dbt-bit.svg)](./deprecations.md)

#### [Deprecations](./deprecations.md)

[Features in your project code (models, YAML, macros) that still work but will be removed. **Impact:** Currently warnings; will cause errors in future versions. **Action:** Update your project code to use the new syntax.](./deprecations.md)

[![](/img/icons/dbt-bit.svg)](./global-configs/behavior-changes.md)

#### [Behavior change flags](./global-configs/behavior-changes.md)

[Settings in your dbt\_project.yml file that let you opt in or out of new behaviors during migration periods. **Impact:** Controls whether dbt uses old or new behavior; defaults change over time. **Action:** Set flags to control timing of adoption.](./global-configs/behavior-changes.md)

[![](/img/icons/square-terminal.svg)](../docs/dbt-versions/dbt-upgrade/upgrading-to-v2.md#deprecated-flags)

#### [Deprecated CLI flags](../docs/dbt-versions/dbt-upgrade/upgrading-to-v2.md#deprecated-flags)

[Command-line flags passed to dbt commands that are being removed in Fusion. **Impact:** Some ignored (with warnings); **--models** flag will error in Fusion. **Action:** Update job definitions and scripts to remove or replace these flags.](../docs/dbt-versions/dbt-upgrade/upgrading-to-v2.md#deprecated-flags)

## Preparing for Fusion

If you're upgrading to Fusion, you should:

* [ ] Resolve all [deprecations](./deprecations.md) to avoid causing errors in Fusion.
* [ ] Review [behavior change flags](./global-configs/behavior-changes.md) to understand how Fusion will behave (new behavior is always enabled).
* [ ] Update [deprecated CLI flags](../docs/dbt-versions/dbt-upgrade/upgrading-to-v2.md#deprecated-flags) to avoid errors in Fusion.

## Related docs

* [Full deprecations list](./deprecations.md)
* [Behavior change flags](./global-configs/behavior-changes.md)
* [Upgrading to Fusion](../docs/dbt-versions/dbt-upgrade/upgrading-to-v2.md)
* [Fusion readiness checklist](../docs/dbt/dbt-readiness.md)
* [Events and logging](./events-logging.md)
