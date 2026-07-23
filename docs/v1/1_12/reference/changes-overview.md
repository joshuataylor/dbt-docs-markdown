# Deprecation & warnings overview

When using dbt, you may see warnings or other changes that need your attention. These changes help us move forward with the latest version of dbt and improve the experience for all users.

Use this page to understand the different types of changes, what to do, and where to find more information.

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/reference/deprecations.md)

#### [Deprecations](https://docs.getdbt.com/reference/deprecations.md)

[Features in your project code (models, YAML, macros) that still work but will be removed.](https://docs.getdbt.com/reference/deprecations.md)

<br />

<br />

[**Impact:** Currently warnings; will cause errors in future versions.](https://docs.getdbt.com/reference/deprecations.md)

<br />

<br />

[**Action:** Update your project code to use the new syntax.](https://docs.getdbt.com/reference/deprecations.md)

[![](/img/icons/dbt-bit.svg)](https://docs.getdbt.com/reference/global-configs/behavior-changes.md)

#### [Behavior change flags](https://docs.getdbt.com/reference/global-configs/behavior-changes.md)

[Settings in your dbt\_project.yml file that let you opt in or out of new behaviors during migration periods.](https://docs.getdbt.com/reference/global-configs/behavior-changes.md)

<br />

<br />

[**Impact:** Controls whether dbt uses old or new behavior; defaults change over time.](https://docs.getdbt.com/reference/global-configs/behavior-changes.md)

<br />

<br />

[**Action:** Set flags to control timing of adoption.](https://docs.getdbt.com/reference/global-configs/behavior-changes.md)

[![](/img/icons/square-terminal.svg)](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md#deprecated-flags)

#### [Deprecated CLI flags](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md#deprecated-flags)

[Command-line flags passed to dbt commands that are being removed in Fusion.](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md#deprecated-flags)

<br />

<br />

[**Impact:** Some ignored (with warnings); **--models** flag will error in Fusion.](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md#deprecated-flags)

<br />

<br />

[**Action:** Update job definitions and scripts to remove or replace these flags.](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md#deprecated-flags)

## Preparing for Fusion[​](#preparing-for-fusion "Direct link to Preparing for Fusion")

If you're upgrading to Fusion, you should:

* <!-- -->
  Resolve all [deprecations](https://docs.getdbt.com/reference/deprecations.md) to avoid causing errors in Fusion.
* <!-- -->
  Review [behavior change flags](https://docs.getdbt.com/reference/global-configs/behavior-changes.md) to understand how Fusion will behave (new behavior is always enabled).
* <!-- -->
  Update [deprecated CLI flags](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md#deprecated-flags) to avoid errors in Fusion.

## Related docs[​](#related-docs "Direct link to Related docs")

* [Full deprecations list](https://docs.getdbt.com/reference/deprecations.md)
* [Behavior change flags](https://docs.getdbt.com/reference/global-configs/behavior-changes.md)
* [Upgrading to Fusion](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md)
* [Fusion readiness checklist](https://docs.getdbt.com/docs/fusion/fusion-readiness.md)
* [Events and logging](https://docs.getdbt.com/reference/events-logging.md)
