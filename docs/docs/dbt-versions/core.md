# About dbt Core versions

dbt Core releases follow [semantic versioning](https://semver.org/) guidelines. For more on how we use semantic versions, see [How dbt Core uses semantic versioning](#how-dbt-core-uses-semantic-versioning).

Release Tracks keep you up to date, always

*Did you know that you can always be working with the latest features and functionality?*

With dbt, you can get early access to new functionality before it becomes available in dbt Core and without the need of managing your own version upgrades. Refer to the [**Latest** release track](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md) setting for details.

dbt Labs provides different support levels for different versions, which may include new features, bug fixes, or security patches:

* **[Active](https://docs.getdbt.com/docs/dbt-versions/core.md#ongoing-patches)** — We will patch regressions, new bugs, and include fixes for older bugs / quality-of-life improvements. We implement these changes when we have high confidence that they're narrowly scoped and won't cause unintended side effects.
* **[Critical](https://docs.getdbt.com/docs/dbt-versions/core.md#ongoing-patches)** — Newer minor versions transition the previous minor version into "Critical Support" with limited "security" releases for critical security and installation fixes.
* **[End of Life](https://docs.getdbt.com/docs/dbt-versions/core.md#eol-version-support)** — Minor versions that have reached EOL no longer receive new patch releases.
* **Deprecated** — dbt Core versions that are no longer maintained by dbt Labs, nor supported in the dbt platform.

### Latest releases[​](#latest-releases "Direct link to Latest releases")

| dbt Core                                                                                                 | Initial release | Support level and end date          |
| -------------------------------------------------------------------------------------------------------- | --------------- | ----------------------------------- |
| [**v1.11**](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v1.11.md)                | Dec 19, 2025    | **Active Support — Dec 18, 2026**   |
| [**v1.10**](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v1.10.md)                | Jun 16, 2025    | **Critical Support — Jun 15, 2026** |
| [**v1.9**](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v1.9.md)                  | Dec 9, 2024     | Deprecated ⛔️                      |
| [**v1.8**](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v1.8.md)                  | May 9, 2024     | Deprecated ⛔️                      |
| [**v1.7**](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v1.7.md)                  | Nov 2, 2023     | End of Life ⚠️                      |
| [**v1.6**](<https://docs.getdbt.com/docs/dbt-versions/core-upgrade/Older versions/upgrading-to-v1.6.md>) | Jul 31, 2023    | End of Life ⚠️                      |
| [**v1.5**](<https://docs.getdbt.com/docs/dbt-versions/core-upgrade/Older versions/upgrading-to-v1.5.md>) | Apr 27, 2023    | End of Life ⚠️                      |
| [**v1.4**](<https://docs.getdbt.com/docs/dbt-versions/core-upgrade/Older versions/upgrading-to-v1.4.md>) | Jan 25, 2023    | End of Life ⚠️                      |
| [**v1.3**](<https://docs.getdbt.com/docs/dbt-versions/core-upgrade/Older versions/upgrading-to-v1.3.md>) | Oct 12, 2022    | End of Life ⚠️                      |
| [**v1.2**](<https://docs.getdbt.com/docs/dbt-versions/core-upgrade/Older versions/upgrading-to-v1.2.md>) | Jul 26, 2022    | Deprecated ⛔️                      |
| [**v1.1**](<https://docs.getdbt.com/docs/dbt-versions/core-upgrade/Older versions/upgrading-to-v1.1.md>) | Apr 28, 2022    | Deprecated ⛔️                      |
| [**v1.0**](<https://docs.getdbt.com/docs/dbt-versions/core-upgrade/Older versions/upgrading-to-v1.0.md>) | Dec 3, 2021     | Deprecated ⛔️                      |
| **v0.X** ⛔️                                                                                             | (Various dates) | Deprecated ⛔️                      |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

All functionality in dbt Core since the v1.7 release is available in [dbt release tracks](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md), which provide automated upgrades at a cadence appropriate for your team.

1 Release tracks are required for the Developer and Starter plans on dbt. Accounts using older dbt versions will be migrated to the **Latest** release track.

For customers of dbt: dbt Labs strongly recommends migrating environments on older and unsupported versions to [release tracks](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md) or a supported version. In 2025, dbt Labs will remove the oldest dbt Core versions from availability in dbt platform, starting with v1.0 -- v1.2.

### Further reading[​](#further-reading "Direct link to Further reading")

* To learn how you can use dbt Core versions in dbt, see [Choosing a dbt Core version](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-version-in-cloud.md).
* To learn about installing dbt Core, see "[How to install dbt Core](https://docs.getdbt.com/docs/core/installation-overview.md)."
* To restrict your project to only work with a range of dbt Core versions, or use the currently running dbt Core version, see [`require-dbt-version`](https://docs.getdbt.com/reference/project-configs/require-dbt-version.md) and [`dbt_version`](https://docs.getdbt.com/reference/dbt-jinja-functions/dbt_version.md).

## Version support prior to v1.0[​](#version-support-prior-to-v10 "Direct link to Version support prior to v1.0")

All dbt Core versions released prior to 1.0 and their version-specific documentation have been deprecated. If upgrading to a currently supported version, reference our [best practices for upgrading](#best-practices-for-upgrading)

## EOL version support[​](#eol-version-support "Direct link to EOL version support")

All dbt Core minor versions that have reached end-of-life (EOL) will have no new patch releases. This means they will no longer receive any fixes, including for known bugs that have been identified. Fixes for those bugs will instead be made in newer minor versions that are still under active support.

We recommend upgrading to a newer version in [dbt](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-version-in-cloud.md) or [dbt Core](https://docs.getdbt.com/docs/core/installation-overview.md#upgrading-dbt-core) to continue receiving support.

All dbt Core v1.0 and later are available in dbt until further notice. In the future, we intend to align dbt availability with dbt Core ongoing support. You will receive plenty of advance notice before any changes take place.

## Current version support[​](#current-version-support "Direct link to Current version support")

### Minor versions[​](#minor-versions "Direct link to Minor versions")

Minor versions include new features and capabilities. They will be supported for one year from their initial release date. *dbt Labs is committed to this 12-month support timeframe.* Our mechanism for continuing to support a minor version is by releasing new patches: small, targeted bug fixes. Whenever we refer to a minor version, such as v1.0, we always mean its latest available patch release (v1.0.x).

While a minor version is officially supported:

* You can use it in dbt. For more on dbt versioning, see [Choosing a dbt version](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-version-in-cloud.md).
* You can select it from the version dropdown on this website, to see documentation that is accurate for use with that minor version.

### Ongoing patches[​](#ongoing-patches "Direct link to Ongoing patches")

During the 12-month support window, we will continue to release new patch versions that include fixes.

**Active Support:** In the first few months after a minor version's initial release, we will patch it with "bugfix" releases. These will include fixes for regressions and net-new bugs that were present in the minor version's original release.

**Critical Support:** When a newer minor version is available, we will transition the previous minor version into "Critical Support." Subsequent patches to that older minor version will be "security" releases only, limited to critical fixes related to security and installation.

After a minor version reaches the end of its critical support period, one year after its initial release, no new patches will be released.

### Future versions[​](#future-versions "Direct link to Future versions")

For the latest information about upcoming releases, including planned release dates and which features and fixes might be included, consult the [`dbt-core` repository milestones](https://github.com/dbt-labs/dbt-core/milestones) and [product roadmaps](https://github.com/dbt-labs/dbt-core/tree/main/docs/roadmap).

## Best practices for upgrading[​](#best-practices-for-upgrading "Direct link to Best practices for upgrading")

Because of our new version practice, we've outlined best practices and expectations for dbt users to upgrade as we continue to release new versions of dbt Core.

### Upgrading to new patch versions[​](#upgrading-to-new-patch-versions "Direct link to Upgrading to new patch versions")

We expect users to upgrade to patches as soon as they're available. When we refer to a "minor version" of dbt Core, such as v1.0, we are always referring to the latest available patch release for that minor version. We encourage you to structure your development and production environments so that you can always install the latest patches of `dbt-core` and any adapter plugins. (Note that patch numbers may be different between dbt-core and plugins. [See below](#how-we-version-adapter-plugins) for an explanation.)

### Upgrading to new minor versions[​](#upgrading-to-new-minor-versions "Direct link to Upgrading to new minor versions")

During the official support period, minor versions will remain available in dbt and the version dropdown on the docs site. While we do not expect users to immediately upgrade to newer minor versions as soon as they're available, there will always be some features and fixes only available for users of the latest minor version.

### Trying prereleases[​](#trying-prereleases "Direct link to Trying prereleases")

All dbt Core versions are available as *prereleases* before the final release. "Release candidates" are available for testing, in production-like environments, two weeks before the final release. For minor versions, we also aim to release one or more "betas," which include new features and invite community feedback, 4+ weeks before the final release. It is in your interest to help us test prereleases—we need your help!

## How dbt Core uses semantic versioning[​](#how-dbt-core-uses-semantic-versioning "Direct link to How dbt Core uses semantic versioning")

Like many software projects, dbt Core releases follow [semantic versioning](https://semver.org/), which defines three types of version releases.

* **Major versions:** To date, dbt Core has had one major version release: v1.0.0. When v2.0.0 is released, it will introduce new features, and functionality that has been announced for deprecation will stop working.
* **Minor versions**, also called "feature" releases, include a mix of new features, behind-the-scenes improvements, and changes to existing capabilities that are **backwards compatible** with previous minor versions. They will not break code in your project that relies on documented functionality.
* **Patch versions**, also called "bugfix" or "security" releases, include **fixes *only***. These fixes could be needed to restore previous (documented) behavior, fix obvious shortcomings of new features, or offer critical fixes for security or installation issues. We are judicious about which fixes are included in patch releases, to minimize the surface area of changes.

We are committed to avoiding breaking changes in minor versions for end users of dbt. There are two types of breaking changes that may be included in minor versions:

* Changes to the Python interface for adapter plugins. These changes are relevant *only* to adapter maintainers, and they will be clearly communicated in documentation and release notes. For more information, refer to [Build, test, document, and promote adapters](https://docs.getdbt.com/guides/adapter-creation.md) guide.
* Changes to metadata interfaces, including [artifacts](https://docs.getdbt.com/docs/deploy/artifacts.md) and [logging](https://docs.getdbt.com/reference/events-logging.md), signalled by a version bump. Those version upgrades may require you to update external code that depends on these interfaces, or to coordinate upgrades between dbt orchestrations that share metadata, such as [state-powered selection](https://docs.getdbt.com/reference/node-selection/syntax.md#about-node-selection).

### How we version adapter plugins[​](#how-we-version-adapter-plugins "Direct link to How we version adapter plugins")

When you use dbt, you use a combination of `dbt-core` and an adapter plugin specific to your database. You can see the current list in [Supported Data Platforms](https://docs.getdbt.com/docs/supported-data-platforms.md). Both `dbt-core` and dbt adapter plugins follow semantic versioning.

`dbt-core` and adapter plugins use the `dbt-adapters` interface to coordinate new features and behind-the-scenes changes. New adapter features are defined in `dbt-adapters` (which `dbt-core` will use). These features are opt-in, meaning they only impact adapters that explicitly implement them. This allows us to independently release adapters, `dbt-adapters`, and `dbt-core` without creating a broken experience for users.

Unlike `dbt-core` versions before 1.8, the minor and patch version numbers might not match between `dbt-core` and the adapter plugin(s) you've installed.

For example, you might find you're using `dbt-core==1.8.0` with `dbt-snowflake==1.9.0`. Even though these don't have the same minor version, they can still work together as they both work with `dbt-adapters==1.8.0`. Patch releases can contain important bug or security fixes so it’s critical to stay up to date.

You can use the `dbt --version` command to see which versions you have installed:

```text
$ dbt --version
Core:
  - installed: 1.8.0
  - latest:    1.8.0 - Up to date!

Plugins:
  - snowflake: 1.9.0 - Up to date!
```

You can see which version of the registered adapter that's being invoked in the [logs](https://docs.getdbt.com/reference/global-configs/logs.md). Below is an example of the message in the `logs/dbt.log` file:

```text
[0m13:13:48.572182 [info ] [MainThread]: Registered adapter: snowflake=1.9.0
```

It's likely that newer patches have become available since then, so it's always important to check and make sure you're up to date!

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
