# Upgrade versions in dbt platform

dbt platformⓘ

In dbt, both [jobs](https://docs.getdbt.com/docs/deploy/jobs.md) and [environments](https://docs.getdbt.com/docs/dbt-platform-environments.md) are configured to use a specific version of dbt Core. The version can be upgraded at any time.

## Environments[​](#environments "Direct link to Environments")

Navigate to the settings page of an environment, then click **Edit**. Click the **dbt version** dropdown bar and make your selection. You can select a [release track](#release-tracks) to receive ongoing updates (recommended), or a legacy version of dbt Core. Be sure to save your changes before navigating away.

[![Example environment settings in dbt](/img/docs/dbt-platform/platform-configuring-dbt-platform/choosing-dbt-version/example-environment-settings.png?v=2 "Example environment settings in dbt")](#)Example environment settings in dbt

### Release Tracks[​](#release-tracks "Direct link to Release Tracks")

Starting in 2024, your project gets upgraded automatically on a cadence that you choose:

The **Latest** track ensures you have up-to-date dbt functionality, and early access to new features of the dbt framework. The **Compatible** and **Extended** tracks are designed for customers who need a less-frequent release cadence, the ability to test new dbt releases before they go live in production, and/or ongoing compatibility with the latest open source releases of dbt Core.

As a best practice, dbt Labs recommends that you test the upgrade in development first; use the [Override dbt version](#override-dbt-version) setting to test *your* project on the latest dbt version before upgrading your deployment environments and the default development environment for all your colleagues.

To upgrade an environment in the [dbt Admin API](https://docs.getdbt.com/docs/dbt-apis/admin-api.md) or [Terraform](https://registry.terraform.io/providers/dbt-labs/dbtcloud/latest), set `dbt_version` to the name of your release track:

* `fusion-nightly`
* `fusion-stable` (formerly `latest-fusion`)
* `fusion-extended`
* `fusion-fallback`
* `latest` (default)
* `compatible` (available to Starter, Enterprise, Enterprise+ plans)
* `extended` (available to all Enterprise plans)

### Override dbt version[​](#override-dbt-version "Direct link to Override dbt version")

Configure your project to use a different dbt version than what's configured in your [development environment](https://docs.getdbt.com/docs/dbt-platform-environments.md#types-of-environments). This *override* only affects your user account, no one else's. Use this to safely test new dbt features before upgrading the dbt version for your projects.

1. Click your account name from the left side panel and select **Account settings**.
2. Choose **Credentials** from the sidebar and select a project. This opens a side panel.
3. In the side panel, click **Edit** and scroll to the **User development settings** section.
4. Choose a version from the **dbt version** dropdown and click **Save**.

An example of overriding the configured version to [**Latest** release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md) for the selected project:

[![Example of overriding the dbt version on your user account](/img/docs/dbt-platform/platform-configuring-dbt-platform/choosing-dbt-version/example-override-version.png?v=2 "Example of overriding the dbt version on your user account")](#)Example of overriding the dbt version on your user account

5. (Optional) Verify that dbt will use your override setting to build the project by invoking a `dbt build` command in the Studio IDE's command bar. Expand the **System Logs** section and find the output's first line. It should begin with `Running with dbt=` and list the version dbt is using.
   <br />
   <br />
   For users on Release tracks, the output will display `Running dbt...` instead of a specific version, reflecting the flexibility and continuous automatic updates provided by the release track functionality.

## dbt Fusion engine[​](#dbt-fusion-engine "Direct link to dbt Fusion engine")

dbt Labs has introduced the new [dbt Fusion engine](https://docs.getdbt.com/docs/fusion/about-fusion.md), a ground-up rebuild of dbt. This is currently generally available for Snowflake projects and in preview for other supported adapters on the dbt platform. Eligible customers can update environments to Fusion using the same workflows as v1.x, but remember:

* If you don't see the `Fusion Stable` release track as an option, you should check with your dbt Labs account team about eligibility.

* To increase the compatibility of your project, update all jobs and environments to the **Latest** release track and read more about the changes in our [upgrade guide](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md).

* Make sure you're using a [supported adapter](https://docs.getdbt.com/docs/platform/connect-data-platform/about-connections.md?version=2.0) and authentication method:

   BigQuery[Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

  * Service Account / User Token
  * Native OAuth
  * External OAuth
    <!-- -->
    * [Workload Identity Federation](https://docs.getdbt.com/docs/platform/manage-access/set-up-bigquery-oauth.md#set-up-bigquery-workload-identity-federation) (Microsoft Entra)
  * [Required permissions](https://docs.getdbt.com/docs/local/connect-data-platform/bigquery-setup.md#required-permissions)

   Databricks[Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

  * Service Account / User Token
  * Native OAuth

   Redshift[Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

  * Username / Password
  * IAM profile

   Snowflake

  * Username / Password
  * Native OAuth
  * External OAuth
  * Key pair using a modern PKCS#8 method
  * MFA

  *Adapter lifecycle can differ between the dbt platform and local development — an adapter can reach GA in the dbt platform before it reaches GA for local use.*

  <br />

* Once you upgrade your development environment(s) to `Fusion Stable`, every user will have to restart the IDE.

  [![Upgrade to the Fusion engine in your environment settings.](/img/docs/dbt-platform/platform-configuring-dbt-platform/platform-upgrading-dbt-versions/upgrade-fusion.png?v=2 "Upgrade to the Fusion engine in your environment settings.")](#)Upgrade to the Fusion engine in your environment settings.

### Upgrading environments to Fusion[​](#upgrading-environments-to-fusion "Direct link to Upgrading environments to Fusion")

When you're ready to upgrade your project(s) to dbt Fusion engine, there are some tools available to you in the dbt platform UI to help you get started. The Fusion upgrade assistant will step you through the process of preparing and upgrading your projects.

[![The Fusion upgrade assistant.](/img/docs/dbt-platform/platform-configuring-dbt-platform/choosing-dbt-version/fusion-upgrade-gui.png?v=2 "The Fusion upgrade assistant.")](#)The Fusion upgrade assistant.

#### Prerequisites[​](#prerequisites "Direct link to Prerequisites")

To take advantage of the upgrade assistant, you'll need to meet the following prerequisites:

* Your dbt project must be updated to use the **Latest** release track.
* You must have a `developer` license.
* You must have the
  <!-- -->
  beta enabled for your account. For more information, please contact your account manager.

#### Assign access to upgrade[​](#assign-access-to-upgrade "Direct link to Assign access to upgrade")

The Fusion readiness & upgrade flow are controlled by two account-level settings that an admin must configure.

Step 1: Enable Fusion readiness features (required)

The upgrade assistant and readiness panel only appear after enabling this setting. From your **Account settings**:

1. Navigate to the **Account** screen.
2. Click **Edit** and scroll to the **Settings** section.
3. Click the box next to **Enable Fusion readiness & upgrade features**.
4. Click **Save**.

Once enabled, all admins and developers can see each project's Fusion readiness status and which jobs are eligible or ineligible for Fusion. Admins can also initiate the Fusion upgrade from development environments, environment settings, and job settings (subject to existing user permissions). Developer-licensed users will have access to debug tools to help make projects Fusion eligible in both development and production environments.

Step 2: Restrict upgrade access (optional, Enterprise/Enterprise+ only)

By default, all admins and developer-licensed users can access the Fusion readiness & upgrade flow. To restrict upgrade execution to users with the `Fusion admin` permission set, enable this additional setting. From your **Account settings**:

1. Navigate to the **Account** screen.
2. Click **Edit** and scroll to the **Settings** section.
3. Click the box next to **Enable restricted Fusion upgrade permissions**.
4. Click **Save**.

This hides the Fusion upgrade workflows from users who don't have the [`Fusion admin`](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions.md#fusion-admin) permission set. To grant access to the upgrade workflows to specific projects and/or specific users:

1. Navigate to an existing group in your **Account settings** and click **Edit**, or click [**Create group**](https://docs.getdbt.com/docs/platform/manage-access/about-user-access.md#create-new-groups) to create a new one.
2. Scroll to the **Access and permissions** section and click **Add permission**.
3. Select the **Fusion admin** permission set from the dropdown, then select the project(s) you want the users to access.
4. Click **Save**.

[![Assign Fusion admin to groups and projects.](/img/docs/dbt-platform/platform-configuring-dbt-platform/choosing-dbt-version/assign-fusion-admin.png?v=2 "Assign Fusion admin to groups and projects.")](#)Assign Fusion admin to groups and projects.

The Fusion upgrade workflows helps identify areas of the project that need to be updated and provides tools for manually resolving and autofixing any errors.

#### Upgrade your development environment[​](#upgrade-your-development-environment "Direct link to Upgrade your development environment")

To begin the process of upgrading to Fusion with the assistant:

1. From the project homepage or sidebar menu, click the **Start Fusion upgrade** or **Get started** button. You will be redirected to the Studio IDE.

[![Start the Fusion upgrade.](/img/docs/dbt-platform/platform-configuring-dbt-platform/choosing-dbt-version/start-upgrade.png?v=2 "Start the Fusion upgrade.")](#)Start the Fusion upgrade.

2. At the top of the Studio IDE click **Check deprecation warnings**.

[![Begin the process of parsing for deprecation warnings.](/img/docs/dbt-platform/platform-configuring-dbt-platform/choosing-dbt-version/check-deprecations.png?v=2 "Begin the process of parsing for deprecation warnings.")](#)Begin the process of parsing for deprecation warnings.

3. dbt parses your project for the deprecations and presents a list of all deprecation warnings along with the option to **Autofix warnings**. Autofixing attempts to correct all syntax errors automatically. See [Fix deprecation warnings](https://docs.getdbt.com/docs/platform/studio-ide/autofix-deprecations.md) for more information.

[![Begin the process of parsing for deprecation warnings.](/img/docs/dbt-platform/platform-configuring-dbt-platform/choosing-dbt-version/check-deprecations.png?v=2 "Begin the process of parsing for deprecation warnings.")](#)Begin the process of parsing for deprecation warnings.

4. Once the deprecation warnings have been resolved, click the **Enable Fusion** button. This upgrades your development environment to Fusion!

[![You're now ready to upgrade to Fusion in your development environment!](/img/docs/dbt-platform/platform-configuring-dbt-platform/choosing-dbt-version/autofix-success.png?v=2 "You're now ready to upgrade to Fusion in your development environment!")](#)You're now ready to upgrade to Fusion in your development environment!

Now that you've upgraded your development environment to <!-- -->, you're ready to start the process of upgrading your Production, Staging, and General environments. Follow your organization's standard procedures and use the [release tracks](#release-tracks) to upgrade.

<!-- -->

Enable the Fusion readiness panel

The Fusion readiness panel shows each project's eligibility status and blockers in the dbt platform. It's rolling out in phases — if it's not enabled for your account yet, an [account admin](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions.md#account-admin) can turn it on in **Account settings** → **Account**. Refer to [Enable Fusion readiness features](https://docs.getdbt.com/guides/prepare-fusion-upgrade.md?step=2) for setup steps.

If you have access to dbt Wizard, use the [dbt Wizard's Fusion migration workflow](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md#fusion-migration-workflow) to help you fix compatibility errors directly from the Studio IDE using dbt Wizard — no manual log investigation needed!

#### Upgrade considerations[​](#upgrade-considerations "Direct link to Upgrade considerations")

Keep in mind the following considerations during the upgrade process:

* **Manifest compatibility** — Fusion produces a `v12` [manifest](https://docs.getdbt.com/reference/artifacts/manifest-json.md) that's compatible with dbt Core. The only differences are optional Fusion-specific fields that only Fusion writes, which dbt Core safely ignores.

  As a result, you can run Fusion and dbt Core side by side. State-dependent features such as `state:modified`, `--defer`, and cross-environment `dbt docs generate` work across mixed Fusion and dbt Core environments, so you can migrate to Fusion incrementally without breaking existing dbt Core jobs.

State-aware orchestration is now dbt State

[dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md) works with all engines and environments: dbt Core, the dbt platform, and dbt Fusion engine.

If you're using state-aware orchestration prior to June 1, 2026, you can continue using it. Existing state-aware orchestration customers automatically receive a 90-day trial of dbt State. To get started, refer to [Migrate from state-aware orchestration](https://docs.getdbt.com/docs/deploy/dbt-state-migration.md).

## Jobs[​](#jobs "Direct link to Jobs")

Each job in dbt can be configured to inherit parameters from the environment it belongs to.

[![Settings of a dbt job](/img/docs/dbt-platform/platform-configuring-dbt-platform/choosing-dbt-version/job-settings.png?v=2 "Settings of a dbt job")](#)Settings of a dbt job

The example job seen in the screenshot above belongs to the environment "Prod". It inherits the dbt version of its environment as shown by the **Inherited from ENVIRONMENT\_NAME (DBT\_VERSION)** selection. You may also manually override the dbt version of a specific job to be any of the current Core releases supported by Cloud by selecting another option from the dropdown.

## Supported versions[​](#supported-versions "Direct link to Supported versions")

dbt Labs has always encouraged users to upgrade dbt Core versions whenever a new minor version is released. We released our first major version of dbt - `dbt 1.0` - in December 2021. Alongside this release, we updated our policy on which versions of dbt Core we will support in the dbt platform.

> **Starting with v1.0, all subsequent minor versions are available in dbt. Versions are actively supported, with patches and bug fixes, for 1 year after their initial release. At the end of the 1-year window, we encourage all users to upgrade to a newer version for better ongoing maintenance and support.**

We provide different support levels for different versions, which may include new features, bug fixes, or security patches:

* **[Active](https://docs.getdbt.com/docs/dbt-versions.md#current-version-support)**: In the first few months after a minor version's initial release, we patch it with bugfix releases. These include fixes for regressions, new bugs, and older bugs / quality-of-life improvements. We implement these changes when we have high confidence that they're narrowly scoped and won't cause unintended side effects.
* **[Critical](https://docs.getdbt.com/docs/dbt-versions.md#current-version-support)**: When a newer minor version ships, the previous one transitions to "Critical Support" for the remainder of its one-year window. Patches during this period are limited to critical security and installation fixes. After the one-year window ends, the version reaches end of life.
* **[End of Life](https://docs.getdbt.com/docs/dbt-versions.md#end-of-life-versions)**: Minor versions that have reached EOL no longer receive new patch releases.
* **Deprecated**: dbt Core versions that are no longer maintained by dbt Labs, nor supported in the dbt platform.

We'll continue to update the following release table so that users know when we plan to stop supporting different versions of Core in dbt.

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

Starting with v1.0, dbt will ensure that you're always using the latest compatible patch release of `dbt-core` and plugins, including all the latest fixes. You may also choose to try prereleases of those patch releases before they are generally available.

For more on version support and future releases, see [Understanding dbt Core versions](https://docs.getdbt.com/docs/dbt-versions.md).

### Need help upgrading?[​](#need-help-upgrading "Direct link to Need help upgrading?")

If you want more advice on how to upgrade your dbt projects, check out our [migration guides](https://docs.getdbt.com/docs/dbt-versions/core-upgrade.md) and our [upgrading Q\&A page](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md#upgrading-legacy-versions-under-10).

### Testing your changes before upgrading[​](#testing-your-changes-before-upgrading "Direct link to Testing your changes before upgrading")

Once you know what code changes you'll need to make, you can start implementing them. We recommend you:

* Create a separate dbt project, "Upgrade project", to test your changes before making them live in your main dbt project.
* In your "Upgrade project", connect to the same repository you use for your production project.
* Set the development environment [settings](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md) to run the latest version of dbt Core.
* Check out a branch `dbt-version-upgrade`, make the appropriate updates to your project, and verify your dbt project compiles and runs with the new version in the Studio IDE.
  <!-- -->
  * If upgrading directly to the latest version results in too many issues, try testing your project iteratively on successive minor versions. There are years of development and a few breaking changes between distant versions of dbt Core (for example, 1.0 --> 1.10). The likelihood of experiencing problems upgrading between successive minor versions is much lower, which is why upgrading regularly is recommended.
* Once you have your project compiling and running on the latest version of dbt in the development environment for your `dbt-version-upgrade` branch, try replicating one of your production jobs to run off your branch's code.
* You can do this by creating a new deployment environment for testing, setting the custom branch to 'ON' and referencing your `dbt-version-upgrade` branch. You'll also need to set the dbt version in this environment to the latest dbt Core version.

[![Setting your testing environment](/img/docs/dbt-platform/platform-configuring-dbt-platform/platform-upgrading-dbt-versions/upgrade-environment.png?v=2 "Setting your testing environment")](#)Setting your testing environment

* Then add a job to the new testing environment that replicates one of the production jobs your team relies on.

  <!-- -->

  * If that job runs smoothly, you should be all set to merge your branch into main.
  * Then change your development and deployment environments in your main dbt project to run off the newest version of dbt Core.
