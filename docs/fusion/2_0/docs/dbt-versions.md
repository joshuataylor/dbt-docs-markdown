# About dbt versions

dbt is available in multiple distributions: the dbt Fusion engine (Rust-based, proprietary), dbt Core v2 (Rust-based, open-source), and dbt Core v1 (Python-based, open-source). dbt Core v1 follows [semantic versioning](https://semver.org/) for the 1.x series; dbt Core v2 and the dbt Fusion engine use the 2.x series. This page explains how versioning works for self-hosted dbt installations.

If you're using the dbt platform (including the dbt CLI), you don't need to manage dbt versions yourself. [Release tracks](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md) automatically keep you up to date and provide early access to new features.

## dbt Fusion engine versioning[​](#dbt-fusion-engine-versioning "Direct link to dbt Fusion engine versioning")

The dbt Fusion engine uses semantic versioning starting with version 2.0. To install or update Fusion, see [Install dbt](https://docs.getdbt.com/docs/local/install-dbt.md?version=2).

### Semantic versioning[​](#semantic-versioning "Direct link to Semantic versioning")

Fusion follows [semantic versioning](https://semver.org/):

* **Major versions** (for example, v2 to v3) may include breaking changes. Deprecated functionality will stop working.
* **Minor versions** (for example, v2.0 to v2.1) add features and are backwards compatible. They will not break project code that relies on documented functionality.
* **Patch versions** (for example, v2.0.0 to v2.0.1) include fixes only: bug fixes, security fixes, or installation fixes.

### Release channels[​](#release-channels "Direct link to Release channels")

Fusion is distributed through release channels during the preview period:

| Channel  | Description                            | Stability                                                           |
| -------- | -------------------------------------- | ------------------------------------------------------------------- |
| `latest` | The stable, "known good" version       | ✅ Recommended for most users                                       |
| `canary` | The latest officially released version | ⚠️ Most recent stable version but still undergoing thorough testing |
| `dev`    | The latest development build           | ❌ May not have passed all tests                                    |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Run `dbt system update` to get the latest stable release, or specify a channel with `dbt system update --version canary`.

For current versions and release history, see [Fusion releases](https://docs.getdbt.com/docs/fusion/fusion-releases.md).

### Checking your version[​](#checking-your-version "Direct link to Checking your version")

Run `dbt --version` to check your installed version:

```text
$ dbt --version
dbt Fusion 2.0.0-preview.126
```

### Further reading[​](#further-reading "Direct link to Further reading")

* [Install Fusion](https://docs.getdbt.com/docs/local/install-dbt.md?version=2): Install or update the dbt Fusion engine.
* [Fusion releases](https://docs.getdbt.com/docs/fusion/fusion-releases.md): View current versions and release history.
* [Get started with Fusion](https://docs.getdbt.com/docs/fusion/get-started-fusion.md): Learn about Fusion features and migration.

## dbt Core versioning[​](#dbt-core-versioning "Direct link to dbt Core versioning")

The dbt Core engine uses semantic versioning for the 1.x release series. To install or update dbt Core, see [Install dbt](https://docs.getdbt.com/docs/local/install-dbt.md?version=1).

* **[Active](https://docs.getdbt.com/docs/dbt-versions.md#current-version-support)**: In the first few months after a minor version's initial release, we patch it with bugfix releases. These include fixes for regressions, new bugs, and older bugs / quality-of-life improvements. We implement these changes when we have high confidence that they're narrowly scoped and won't cause unintended side effects.
* **[Critical](https://docs.getdbt.com/docs/dbt-versions.md#current-version-support)**: When a newer minor version ships, the previous one transitions to "Critical Support" for the remainder of its one-year window. Patches during this period are limited to critical security and installation fixes. After the one-year window ends, the version reaches end of life.
* **[End of Life](https://docs.getdbt.com/docs/dbt-versions.md#end-of-life-versions)**: Minor versions that have reached EOL no longer receive new patch releases.
* **Deprecated**: dbt Core versions that are no longer maintained by dbt Labs, nor supported in the dbt platform.

### Latest releases[​](#latest-releases "Direct link to Latest releases")

| dbt Core                                                                                                 | Initial release | Support level and end date                                                            |
| -------------------------------------------------------------------------------------------------------- | --------------- | ------------------------------------------------------------------------------------- |
| [**v2.0**](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md)                    | TBD             | Currently in [alpha](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles.md) |
| [**v1.12**](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v1.12.md)                | Jul 16, 2026    | **Active Support — Jul 15, 2027**                                                     |
| [**v1.11**](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v1.11.md)                | Dec 19, 2025    | **Active Support — Dec 18, 2026**                                                     |
| [**v1.10**](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v1.10.md)                | Jun 16, 2025    | Deprecated ⛔️                                                                        |
| [**v1.9**](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v1.9.md)                  | Dec 9, 2024     | Deprecated ⛔️                                                                        |
| [**v1.8**](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v1.8.md)                  | May 9, 2024     | Deprecated ⛔️                                                                        |
| [**v1.7**](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v1.7.md)                  | Nov 2, 2023     | End of Life ⚠️                                                                        |
| [**v1.6**](<https://docs.getdbt.com/docs/dbt-versions/core-upgrade/Older versions/upgrading-to-v1.6.md>) | Jul 31, 2023    | End of Life ⚠️                                                                        |
| [**v1.5**](<https://docs.getdbt.com/docs/dbt-versions/core-upgrade/Older versions/upgrading-to-v1.5.md>) | Apr 27, 2023    | End of Life ⚠️                                                                        |
| [**v1.4**](<https://docs.getdbt.com/docs/dbt-versions/core-upgrade/Older versions/upgrading-to-v1.4.md>) | Jan 25, 2023    | End of Life ⚠️                                                                        |
| [**v1.3**](<https://docs.getdbt.com/docs/dbt-versions/core-upgrade/Older versions/upgrading-to-v1.3.md>) | Oct 12, 2022    | End of Life ⚠️                                                                        |
| [**v1.2**](<https://docs.getdbt.com/docs/dbt-versions/core-upgrade/Older versions/upgrading-to-v1.2.md>) | Jul 26, 2022    | Deprecated ⛔️                                                                        |
| [**v1.1**](<https://docs.getdbt.com/docs/dbt-versions/core-upgrade/Older versions/upgrading-to-v1.1.md>) | Apr 28, 2022    | Deprecated ⛔️                                                                        |
| [**v1.0**](<https://docs.getdbt.com/docs/dbt-versions/core-upgrade/Older versions/upgrading-to-v1.0.md>) | Dec 3, 2021     | Deprecated ⛔️                                                                        |
| **v0.X** ⛔️                                                                                             | (Various dates) | Deprecated ⛔️                                                                        |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

All functionality in dbt Core since the v1.7 release is available in [dbt release tracks](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md), which provide automated upgrades at a cadence appropriate for your team.

1 Release tracks are required for the Developer and Starter plans on dbt. Accounts using older dbt versions will be migrated to the **Latest** release track.

For customers of dbt: dbt Labs strongly recommends migrating environments on older and unsupported versions to [release tracks](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md) or a supported version. In 2025, dbt Labs will remove the oldest dbt Core versions from availability in dbt platform, starting with v1.0 -- v1.2.

### How dbt Core uses semantic versioning[​](#how-dbt-core-uses-semantic-versioning "Direct link to How dbt Core uses semantic versioning")

dbt follows [semantic versioning](https://semver.org/):

* **Major versions** (for example, v1 to v2) may include breaking changes. Deprecated functionality will stop working.
* **Minor versions** (for example, v1.8 to v1.9) add features and are backwards compatible. They will not break project code that relies on documented functionality.
* **Patch versions** (for example, v1.8.0 to v1.8.1) include fixes only: bug fixes, security fixes, or installation fixes.

We are committed to avoiding breaking changes in minor versions for end users of dbt. There are two types of breaking changes that may be included in minor versions:

* Changes to the Python interface for adapter plugins. These changes are relevant only to adapter maintainers, and they will be clearly communicated in documentation and release notes. For more information, refer to [Build, test, document, and promote adapters guide](https://docs.getdbt.com/guides/adapter-creation.md).

* Changes to metadata interfaces, including [artifacts](https://docs.getdbt.com/docs/deploy/artifacts.md) and [logging](https://docs.getdbt.com/reference/events-logging.md), signalled by a version bump. Those version upgrades may require you to update external code that depends on these interfaces, or to coordinate upgrades between dbt orchestrations that share metadata, such as [state-powered selection](https://docs.getdbt.com/reference/node-selection/syntax.md#about-node-selection).

### Adapter plugin versions[​](#adapter-plugin-versions "Direct link to Adapter plugin versions")

dbt releases `dbt-core` and adapter plugins (such as `dbt-snowflake`) independently. Their minor and patch version numbers may not match, but they coordinate through the `dbt-adapters` interface so you won't get a broken experience. For example, `dbt-core==1.8.0` can work with `dbt-snowflake==1.9.0`.

If you're building or maintaining an adapter, refer to the [adapter creation guide](https://docs.getdbt.com/guides/adapter-creation.md) for details on the `dbt-adapters` interface.

Run `dbt --version` to check your installed versions:

```text
$ dbt --version
Core:
  - installed: 1.8.0
  - latest:    1.8.0 - Up to date!

Plugins:
  - snowflake: 1.9.0 - Up to date!
```

You can also find the registered adapter version in [logs](https://docs.getdbt.com/reference/global-configs/logs.md). For example, in `logs/dbt.log`:

```text
[0m13:13:48.572182 [info ] [MainThread]: Registered adapter: snowflake=1.9.0
```

Refer to [Supported data platforms](https://docs.getdbt.com/docs/supported-data-platforms.md) for the full list of adapters.

### Further reading[​](#further-reading-1 "Direct link to Further reading")

* [Choosing a dbt Core version in dbt](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md): Learn how to use dbt Core versions in dbt.
* [Install dbt Core](https://docs.getdbt.com/docs/local/install-dbt.md?version=1): Install or update dbt Core.
* [`require-dbt-version`](https://docs.getdbt.com/reference/project-configs/require-dbt-version.md) and [`dbt_version`](https://docs.getdbt.com/reference/dbt-jinja-functions/dbt_version.md): Restrict your project to work with a specific range of versions.

## End-of-life versions[​](#end-of-life-versions "Direct link to End-of-life versions")

Once a dbt version reaches end-of-life (EOL), it no longer receives patches, including for known bugs. We recommend upgrading to a newer version in [dbt](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md), [Fusion](https://docs.getdbt.com/docs/local/install-dbt.md?version=2), or [dbt Core](https://docs.getdbt.com/docs/local/install-dbt.md). All versions prior to v1.0 have been deprecated.

## Current version support[​](#current-version-support "Direct link to Current version support")

dbt supports each minor version (for example, v1.8) for *one year* from its initial release. During that window, we release patches with bug fixes and security updates. When we refer to a minor version, we mean its latest available patch (v1.8.x).

After a newer minor version ships, the previous one transitions to **critical support** (security and installation fixes only) for the remainder of its one-year window. After the one-year window ends, the version reaches **end of life** and no longer receives patches.

While a minor version is officially supported:

* You can use it in dbt. For more on dbt versioning, see [Choosing a dbt version](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md).
* You can select it from the version dropdown on this website to see documentation that is accurate for use with that minor version.

## Upgrading[​](#upgrading "Direct link to Upgrading")

Upgrade to new patch versions as soon as they're available. Upgrade to new minor versions when you're ready because you can only get some features and fixes on the latest minor version.

dbt makes all versions available as prereleases before the final release. For minor versions, we aim to release one or more betas 4+ weeks before the final release so you can try new features and share feedback. Release candidates are available about two weeks before the final release for testing in production-like environments. Refer to the [`dbt-core` milestones](https://github.com/dbt-labs/dbt-core/milestones) for details.
