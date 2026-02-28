# Fusion readiness checklist

The dbt Fusion engine is here! We currently offer it as a [private preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles.md#the-dbt-platform) on the dbt platform. Even if we haven't enabled it for your account, you can still start preparing your projects for upgrade. Use this checklist to ensure a smooth upgrade once Fusion becomes available. If this is all new to you, first [learn about Fusion](https://docs.getdbt.com/docs/fusion.md), its current state, and the features available.

## Preparing for Fusion[​](#preparing-for-fusion "Direct link to Preparing for Fusion")

Use the following checklist to prepare your projects for the dbt Fusion engine

### Upgrade to the latest dbt version[​](#upgrade-to-the-latest-dbt-version "Direct link to Upgrade to the latest dbt version")

The **Latest** [release track](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md) has all of the most recent features to help you prepare for Fusion.

* <!-- -->
  Make sure all your projects are on the **Latest** release track across all deployment environments and jobs. This will ensure the simplest, most predictable experience by allowing you to pre-validate that your project doesn't rely on deprecated behaviors.

### Resolve all deprecation warnings[​](#resolve-all-deprecation-warnings "Direct link to Resolve all deprecation warnings")

You must resolve deprecations while your projects are on a dbt Core release track, as they result in warnings that will become errors once you upgrade to Fusion. The autofix tool can automatically resolve many deprecations (such as moving arbitrary configs into the meta dictionary). For a full list of deprecations and how to resolve them, refer to [Deprecations](https://docs.getdbt.com/reference/deprecations.md).

Start a new branch to begin resolving deprecation warnings using one of the following methods:

* [ ] **Run autofix in the dbt platform:** You can address deprecation warnings using the [autofix tool in the Studio IDE](https://docs.getdbt.com/docs/cloud/studio-ide/autofix-deprecations.md). You can run the autofix tool on the **Compatible** or **Latest** release track.
* [ ] **Run autofix locally:** Use the [VS Code extension](https://docs.getdbt.com/docs/about-dbt-extension.md). The extension has a built-in ["Getting Started" workflow](https://docs.getdbt.com/docs/install-dbt-extension.md#getting-started) that will debug your dbt project in the VS Code or Cursor IDE and execute the autofix tool. This has the added benefit of installing Fusion to your computer so you can begin testing locally before implementing in your dbt platform account.
* [ ] **Run autofix locally (without the extension):** Visit the autofix [GitHub repo](https://github.com/dbt-labs/dbt-autofix) to run the tool locally if you're not using VS Code or Cursor. This will only run the tool, it will not install Fusion.

### Validate and upgrade your dbt packages[​](#validate-and-upgrade-your-dbt-packages "Direct link to Validate and upgrade your dbt packages")

The most commonly used dbt Labs managed packages (such as `dbt_utils` and `dbt_project_evaluator`) are already compatible with Fusion, as are a large number of external and community packages. Review [the dbt package hub](https://hub.getdbt.com) to see verified Fusion-compatible packages by checking that the `require-dbt-version` configuration includes `2.0.0` or higher. Refer to [package support](https://docs.getdbt.com/docs/fusion/supported-features.md#package-support) for more information.

* <!-- -->
  Make sure that all of your packages are upgraded to the most recent version, many of which contain enhancements to support Fusion.
* <!-- -->
  Check package repositories to make sure they're compatible with Fusion. If a package you use is not yet compatible, we recommend opening an issue with the maintainer, making the contribution yourself, or removing the package temporarily before you upgrade.

### Validate user-defined functions[​](#validate-user-defined-functions "Direct link to Validate user-defined functions")

### Validate support for functions[​](#validate-support-for-functions "Direct link to Validate support for functions")

Check that Fusion supports all user-defined functions (UDFs) in your project.

Fusion supports nearly all built-in data platform functions out of the box. However, data platforms continuously add new functions that Fusion may not yet support.

If you see the error `dbt0209: No function <function name>`, the resolution depends on whether the function is a UDF or a built-in function:

If you see this warning:

* [ ] **For custom UDFs:** Recreate it as a [native dbt UDF](https://docs.getdbt.com/docs/build/udfs.md#defining-udfs-in-dbt) to get the full Fusion experience. With `static_analysis: baseline` (the default), most UDFs will work out of the box.
* [ ] **For Warehouse-native functions:** Submit a [GitHub issue](https://github.com/dbt-labs/dbt-fusion). Fusion's `baseline` mode handles most cases, but will throw warnings and not errors. You can set `static_analysis: off` for specific models if needed.

### Check for known Fusion limitations[​](#check-for-known-fusion-limitations "Direct link to Check for known Fusion limitations")

Your project may implement features that Fusion currently [limits](https://docs.getdbt.com/docs/fusion/supported-features.md#limitations) or doesn't support.

* <!-- -->
  Remove unnecessary features from your project to make it Fusion compatible.
* <!-- -->
  Monitor progress for critical features, knowing we are working to bring them to Fusion. You can monitor their progress using the issues linked in the [limitations table](https://docs.getdbt.com/docs/fusion/supported-features.md#limitations).

### Review jobs configured in the dbt platform[​](#review-jobs-configured-in-the-dbt-platform "Direct link to Review jobs configured in the dbt platform")

We determine Fusion eligibility using data from your job runs.

* <!-- -->
  Ensure you have at least one job running in each of your projects in the dbt platform.
* <!-- -->
  Ensure all jobs are running on the [**Latest** release track](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md#which-release-tracks-are-available).
* <!-- -->
  Resolve any job failures — all jobs must run successfully for eligibility checks to work.
* <!-- -->
  Delete any jobs that are no longer in use to ensure accurate eligibility reporting.
* <!-- -->
  Make sure you've promoted the changes for deprecation resolution and package upgrades to your git branches that map to your deployment environments.

### Stay informed about Fusion progress[​](#stay-informed-about-fusion-progress "Direct link to Stay informed about Fusion progress")

The dbt Fusion engine remains in private preview and we currently offer it for eligible projects! We will notify you when all your projects are ready for Fusion based on our eligibility checks on your deployment jobs. In the meantime, keep up-to-date with these resources:

* <!-- -->
  Check out the [Fusion homepage](https://www.getdbt.com/product/fusion) for available resources, including supported adapters, prerequisites, installation instructions, limitations, and deprecations.
* <!-- -->
  Read the [Upgrade guide](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-fusion.md) to learn about the new features and functionality that impact your dbt projects.
* <!-- -->
  Monitor progress and get insight into the development process by reading the [Fusion Diaries](https://github.com/dbt-labs/dbt-fusion/discussions/categories/announcements).
* <!-- -->
  Catch up on the [cost savings potential](https://www.getdbt.com/blog/announcing-state-aware-orchestration) of Fusion-powered [state-aware orchestration](https://docs.getdbt.com/docs/deploy/state-aware-about) (hint: 30%+ reduction in warehouse spend!)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
