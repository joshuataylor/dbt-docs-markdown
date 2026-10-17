# Upgrading to v1.13 [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Available in v1

## Resources

* [dbt v1.13 changelog](https://github.com/dbt-labs/dbt/blob/1.13.latest/CHANGELOG.md)
* [dbt v1 CLI Installation guide](../../local/install-dbt.md)
* [dbt platform upgrade guide](../upgrade-dbt-platform-version.md)

## What to know before upgrading

dbt Labs is committed to providing backward compatibility for all versions 1.x. Any behavior changes will be accompanied by a [behavior change flag](../../../reference/global-configs/behavior-changes.md#behavior-change-flags) to provide a migration window for existing projects. If you encounter an error upon upgrading, please let us know by [opening an issue](https://github.com/dbt-labs/dbt/issues/new).

dbt provides the functionality from new versions of dbt v1 via [release tracks](../dbt-release-tracks.md) with automatic upgrades. If you have selected the **v1 Latest** release track in dbt, you already have access to all the features, fixes, and other functionality included in the latest dbt v1 version! If you have selected the **v1 Compatible** release track, you will have access to the next monthly **v1 Compatible** release after the dbt v1.13 final release.

## New and changed features and functionality

### dbt State

[dbt State](../../deploy/dbt-state-about.md) makes dbt smarter about what to build — instead of rebuilding every node on every run, dbt reuses nodes by cloning from another location or skipping a rebuild when the logic and data haven't changed. dbt State is natively available in dbt v1.13. To get started, refer to [Setting up dbt State](../../deploy/dbt-state-setup.md).

## Adapter-specific features and functionalities

**Coming soon**
