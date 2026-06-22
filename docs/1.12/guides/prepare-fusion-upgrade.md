# Upgrade to Fusion part 1: Preparing to upgrade

This guide helps you prepare for an in-place upgrade from dbt Core to the dbt Fusion engine in the dbt platform.

[Back to guides](https://docs.getdbt.com/guides.md)

dbt Fusion engine

dbt platform

Upgrade

Intermediate

[Menu ]()



## Introduction[​](#introduction "Direct link to Introduction")

The dbt Fusion engine represents the next evolution of data transformation. dbt has been rebuilt from the ground up but at its most basic, Fusion is a new version, and like any new version you should take steps to prepare to upgrade. This guide will take you through those preparations.

If Fusion is brand new to you, take a look at our [comprehensive documentation](https://docs.getdbt.com/docs/fusion.md) on what it is, how it behaves, and what's different from dbt Core before getting started with this guide. Once you're caught up, it's time to begin preparing your projects for the speed and power that Fusion has to offer.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

This guide will cover the preparations for upgrading to the dbt Fusion engine and is intended for customers already using the dbt platform with a version of dbt Core. If you're brand new to dbt, check out our [quickstart guides](https://docs.getdbt.com/guides.md).

To follow the steps in this guide, you must meet the following prerequisites:

* You're using a dbt platform account on any tier.

* You have a developer license.

* You have [proper permissions](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions.md) to edit projects.

* Your project is using a Fusion supported adapter:

  <!-- -->

   BigQuery[Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

  * Service Account / User Token
  * Native OAuth
  * External OAuth
    <!-- -->
    * [Workload Identity Federation](https://docs.getdbt.com/docs/platform/manage-access/set-up-bigquery-oauth.md#set-up-bigquery-workload-identity-federation) (Microsoft Entra)
  * [Required permissions](https://docs.getdbt.com/docs/local/connect-data-platform/bigquery-setup.md#required-permissions)

   Databricks[Private preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

  * Service Account / User Token
  * Native OAuth

   Redshift[Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

  * Username / Password
  * IAM profile

   Salesforce Data 360

  * JSON Web Token (JWT) bearer authentication

   Snowflake

  * Username / Password
  * Native OAuth
  * External OAuth
  * Key pair using a modern PKCS#8 method
  * MFA

   Apache Spark (Fusion CLI only)[Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

  * Thrift

    <!-- -->

    * Simple Authentication and Security Layer (SASL) PLAIN
    * No SASL (NOSASL)

  * Livy

    <!-- -->

    * Basic authentication (username and password)
    * When deployed on Amazon Web Services (AWS): AWS Signature Version 4
      <!-- -->
      * Supports authentication using single sign-on, service accounts, or user tokens

   DuckDB (Fusion CLI only)[Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

  DuckDB does not require authentication — it runs locally on your machine.

Upgrading your first project

Start with smaller, newer, or more familiar projects first. This makes it easier to identify and troubleshoot any issues before upgrading larger, more complex projects.

## Enable Fusion readiness features[​](#enable-fusion-readiness-features "Direct link to Enable Fusion readiness features")

The Fusion readiness panel in the dbt platform and shows each project's eligibility status and blockers, is being rolled out in phases. If it hasn't been automatically enabled for your account yet, an [account admin](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions.md#account-admin) can manually enable it. This lets admins and developers see which projects and jobs are eligible for Fusion, identify blockers, and initiate the upgrade once preparation is complete.

### Step 1: Enable the readiness toggle[​](#step-1-enable-the-readiness-toggle "Direct link to Step 1: Enable the readiness toggle")

This step requires account admin access in dbt platform:

1. Click your account name in the left sidebar and select **Account settings**.
2. Navigate to the **Account** screen and click **Edit**.
3. Scroll to the **Settings** section and select the checkbox next to **Enable Fusion readiness & upgrade features**.
4. Click **Save**.

Once enabled:

* All admins and developers can see each project's Fusion readiness status, including which jobs are eligible or ineligible for Fusion and why.
* Admins can initiate the Fusion upgrade from development environments, environment settings, and job settings (subject to existing permissions).

### Step 2: Restrict upgrade access (optional)[​](#step-2-restrict-upgrade-access-optional "Direct link to Step 2: Restrict upgrade access (optional)")

By default, any user who can see the upgrade assistant can use it to initiate an upgrade. To limit upgrade execution to designated users, you can enable an additional access control toggle.

Enterprise tier accounts only

The **Enable restricted Fusion upgrade permissions** toggle is only available to Enterprise/Enterprise+ accounts that have been granted this entitlement. Contact your account manager if you need this capability.

1. In the same **Account** settings screen, select the checkbox next to **Enable restricted Fusion upgrade permissions**.
2. Click **Save**.

When enabled, only users assigned the [`Fusion admin`](https://docs.getdbt.com/docs/platform/manage-access/enterprise-permissions.md#fusion-admin) permission set (scoped to specific projects) can execute the upgrade. For instructions on assigning this permission, refer to [Assign upgrade access](https://docs.getdbt.com/guides/upgrade-to-fusion.md?step=3#assign-upgrade-access-optional) in Part 2 of this guide.

### The Fusion readiness panel[​](#the-fusion-readiness-panel "Direct link to The Fusion readiness panel")

With the readiness experience enabled, you can monitor your project's eligibility as you work through the preparation steps below. The panel shows which jobs are eligible or ineligible for Fusion and why.

[![The Fusion readiness checklist](/img/fusion/fusion-readiness.png?v=2 "The Fusion readiness checklist")](#)The Fusion readiness checklist

Common ineligibility reasons include:

* Environment(s) not on the **Latest** \[release track]\(/docs/dbt-versions/dbt-release-tracks#which-release-tracks-are-available]
* Not using a [supported data platform](https://docs.getdbt.com/docs/fusion/supported-features.md?version=2.0#requirements)
* Project doesn't have at least one successful job run
* Jobs that haven't run in the last 7 days or have recent failures

As you complete the steps in this guide, check the readiness panel to see your eligibility improve.

## Upgrade to the latest dbt Core version[​](#upgrade-to-the-latest-dbt-core-version "Direct link to Upgrade to the latest dbt Core version")

Before upgrading to Fusion, you need to move your environments to the **Latest** [dbt Core release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md). The **Latest** track includes all the features and tooling to help you prepare for Fusion. It ensures the smoothest upgrade experience by validating that your project doesn't rely on deprecated behaviors.

Test before you deploy

Always test version upgrades in development first. Use the [Override dbt version](#step-1-test-in-development-using-override) feature to safely try the **Latest** release track without affecting your team or production runs.

### Step 1: Test in development (using override)[​](#step-1-test-in-development-using-override "Direct link to Step 1: Test in development (using override)")

Test the **Latest** release track for your individual account without changing the environment for your entire team:

1. Click your account name in the left sidebar and select **Account settings**.
2. Select **Credentials** from the sidebar and choose your project.
3. In the side panel, click **Edit** and scroll to **User development settings**.
4. Select **Latest** from the **dbt version** dropdown and click **Save**.

[![Override dbt version in your account settings](/img/docs/dbt-platform/platform-configuring-dbt-platform/choosing-dbt-version/example-override-version.png?v=2 "Override dbt version in your account settings")](#)Override dbt version in your account settings

5. Launch the Studio IDE or dbt CLI and test your normal development workflows.
6. Verify the override is active by running any dbt command and checking the **System Logs**. The first line should show `Running with dbt=` and your selected version. If the version number is `v1.11` or higher, you're on the right path to Fusion readiness.

If everything works as expected, proceed to the next step to start upgrading your environments. If you encounter deprecation warnings, don't fear! We'll address those [later in this guide](https://docs.getdbt.com/guides/prepare-fusion-upgrade.md?step=4). If you encounter errors, revert to your previous version and refer to the [version upgrade guides](https://docs.getdbt.com/docs/dbt-versions/core-upgrade.md) to resolve any differences between your current version and the latest available dbt Core version.

### Step 2: Upgrade your development environment[​](#step-2-upgrade-your-development-environment "Direct link to Step 2: Upgrade your development environment")

After successfully testing your individual development environment with the override, upgrade the development environment for the entire project (be sure to give your team notice!):

1. Navigate to **Environments** in your project settings.
2. Select your **Development** environment and click **Edit**.
3. Click the **dbt version** dropdown and select **Latest**.
4. Click **Save** to apply the changes.

[![Upgrade development environment to Latest dbt Core release track](/img/docs/dbt-platform/platform-configuring-dbt-platform/choosing-dbt-version/select-development.png?v=2 "Upgrade development environment to Latest dbt Core release track")](#)Upgrade development environment to Latest dbt Core release track

Remove your override

Once your development environment is upgraded, you can remove your personal override by returning to your account credentials and selecting the same version as your environment.

### Step 3: Upgrade staging and pre-production[​](#step-3-upgrade-staging-and-pre-production "Direct link to Step 3: Upgrade staging and pre-production")

If your organization has staging or pre-production environments, upgrade these before production:

1. Navigate to **Environments** and select your staging/pre-production environment.
2. Click **Edit** and select **Latest** from the **dbt version** dropdown.
3. Click **Save**.
4. Run your jobs in this environment for a few days to validate everything works correctly.

This provides a final validation layer before upgrading production environments.

### Step 4: Upgrade your production environment[​](#step-4-upgrade-your-production-environment "Direct link to Step 4: Upgrade your production environment")

After validating in staging (or development if you don't have staging), upgrade your production environment:

1. Navigate to **Environments** and select your **Production** environment.
2. Click **Edit** and select **Latest** from the **dbt version** dropdown.
3. Click **Save** to apply the changes.
4. Monitor your first few production runs to ensure everything executes successfully.

### Step 5: Update jobs[​](#step-5-update-jobs "Direct link to Step 5: Update jobs")

While environments control the dbt version for most scenarios, some older job configurations may have version overrides. Review your jobs and [update any that specify a dbt version](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md#jobs) to ensure they use the environment's Latest release track.

## Resolve all deprecation warnings[​](#resolve-all-deprecation-warnings "Direct link to Resolve all deprecation warnings")

Fusion enforces strict validation and won't accept deprecated code that currently generates warnings in dbt Core. You must resolve all deprecation warnings before upgrading to Fusion. Fortunately, the autofix tool in the Studio IDE can automatically resolve most common deprecations for you.

VS Code extension

This guide provides steps to resolve deprecation warnings without leaving dbt platform. If you prefer to work in the VS Code or Cursor editors locally, you can run the autofix in our dbt VS Code extension. Check out the [installation guide](https://docs.getdbt.com/docs/install-dbt-extension.md) for more information about those workflows.

### What the autofix tool handles[​](#what-the-autofix-tool-handles "Direct link to What the autofix tool handles")

The autofix tool can resolve many deprecations automatically, including:

* Moving custom configurations into the `meta` dictionary
* Fixing duplicate YAML keys
* Correcting unrecognized resource properties
* Updating deprecated configuration patterns

Check out the [autofix readme](https://github.com/dbt-labs/dbt-autofix/) for a complete list of the deprecations it addresses.

Fusion package compatibility

In addition to deprecations, the autofix tool attempts to upgrade packages to the lowest supported Fusion-compatible version. Check out [package support](https://docs.getdbt.com/docs/fusion/supported-features.md#package-support) for more information about Fusion compatibility.

### Step 1: Create a new branch[​](#step-1-create-a-new-branch "Direct link to Step 1: Create a new branch")

Before running the autofix tool, create a new branch to isolate your changes:

1. Navigate to the Studio IDE by clicking **Studio** in the left-side menu.
2. Click the **Version control** panel (git branch icon) on the left sidebar.
3. Click **Create branch** and name it something descriptive like `fusion-deprecation-fixes`.
4. Click **Create** to switch to your new branch.

Save before autofixing

The autofix tool will modify files in your project. Make sure to commit or stash any unsaved work to avoid losing changes.

### Step 2: Run the autofix tool[​](#step-2-run-the-autofix-tool "Direct link to Step 2: Run the autofix tool")

Now you're ready to scan for and automatically fix deprecation warnings:

1. Click the **three-dot menu** in the bottom right corner of the Studio IDE.
2. Select **Check & fix deprecations**.

[![Access the Studio IDE options menu](/img/docs/dbt-platform/platform-ide/ide-options-menu-with-save.png?v=2 "Access the Studio IDE options menu")](#)Access the Studio IDE options menu

The tool runs `dbt parse --show-all-deprecations --no-partial-parse` to identify all deprecations in your project. This may take a few moments depending on your project size.

3. When parsing completes, view the results in the **Command history** panel in the bottom left.

[![View command history and deprecation results](/img/docs/dbt-platform/platform-ide/command-history.png?v=2 "View command history and deprecation results")](#)View command history and deprecation results

### Step 3: Review and apply autofixes[​](#step-3-review-and-apply-autofixes "Direct link to Step 3: Review and apply autofixes")

After the deprecation scan completes, review the findings and apply automatic fixes:

1. In the **Command history** panel, review the list of deprecation warnings.
2. Click the **Autofix warnings** button to proceed.

[![Click Autofix warnings to resolve deprecations automatically](/img/docs/dbt-platform/platform-ide/autofix-button.png?v=2 "Click Autofix warnings to resolve deprecations automatically")](#)Click Autofix warnings to resolve deprecations automatically

3. In the **Proceed with autofix** dialog, review the warning and click **Continue**.

[![Confirm autofix operation](/img/docs/dbt-platform/platform-ide/proceed-with-autofix.png?v=2 "Confirm autofix operation")](#)Confirm autofix operation

The tool automatically modifies your project files to resolve fixable deprecations, then runs another parse to identify any remaining warnings.

4. When complete, a success message appears. Click **Review changes**.

[![Autofix complete](/img/docs/dbt-platform/platform-ide/autofix-success.png?v=2 "Autofix complete")](#)Autofix complete

### Step 4: Verify the changes[​](#step-4-verify-the-changes "Direct link to Step 4: Verify the changes")

Review the changes made by the autofix tool to ensure they're correct:

1. Open the **Version control** panel to view all modified files.
2. Click on individual files to review the specific changes.
3. Look for files with moved configurations, corrected properties, or updated syntax.
4. If needed, make any additional manual adjustments.

### Step 5: Commit your changes[​](#step-5-commit-your-changes "Direct link to Step 5: Commit your changes")

Once you're satisfied with the autofix changes, commit them to your branch:

1. In the **Version control** panel, add a descriptive commit message like "Fix deprecation warnings for Fusion upgrade".
2. Click **Commit and sync** to save your changes.

### Step 6: Address remaining deprecations[​](#step-6-address-remaining-deprecations "Direct link to Step 6: Address remaining deprecations")

If the autofix tool reports remaining deprecation warnings that couldn't be automatically fixed:

1. Review the warning messages in the **Command history** panel. Each warning includes the file path and line number.

2. Manually update the code based on the deprecation guidance:

   <!-- -->

   * Custom inputs should be moved to the `meta` config.
   * Deprecated properties should be updated to their new equivalents.
   * Refer to specific [version upgrade guides](https://docs.getdbt.com/docs/dbt-versions/core-upgrade.md) for detailed migration instructions.

3. After making manual fixes, run **Check & fix deprecations** again to verify all warnings are resolved.

4. Commit your changes.

### Step 7: Merge to your main branch[​](#step-7-merge-to-your-main-branch "Direct link to Step 7: Merge to your main branch")

Once all deprecations are resolved:

1. Create a pull request in your git provider to merge your deprecation fixes.
2. Have your team review the changes.
3. Merge the PR to your main development branch.
4. Ensure these changes are deployed to your environments before proceeding with the Fusion upgrade.

## Validate and upgrade your dbt packages[​](#validate-and-upgrade-your-dbt-packages "Direct link to Validate and upgrade your dbt packages")

Run autofix first

This section contains instructions for manual package upgrades. We recommend running the autofix tool before taking these steps.

The autofix tool finds packages incompatible with Fusion and upgrades them to the lowest compatible version. For more information, check out [package support](https://docs.getdbt.com/docs/fusion/supported-features.md#package-support).

dbt packages extend your project's functionality, but they must be compatible with Fusion. Most commonly used packages from dbt Labs (like `dbt_utils` and `dbt_project_evaluator`) and many community packages [already support Fusion](https://docs.getdbt.com/docs/fusion/supported-features.md#package-support). Before upgrading, verify your packages are compatible and upgrade them to the latest versions. Check for packages that support version 2.0.0, or ask the maintainer if you're unsure.

What if a package isn't compatible?

If a critical package isn't yet compatible with Fusion:

* Check with the package maintainer about their roadmap.
* Open an issue requesting Fusion support.
* Consider contributing the compatibility updates yourself.
* Try it out anyway! The incompatible portion of the package might not impact your project.

#### Package compatibility messages[​](#package-compatibility-messages "Direct link to Package compatibility messages")

Inconsistent Fusion warnings and `dbt-autofix` logs

Fusion warnings and `dbt-autofix` logs may show different messages about package compatibility.

If you use [`dbt-autofix`](https://github.com/dbt-labs/dbt-autofix) while upgrading to Fusion in the Studio IDE or dbt VS Code extension, you may see different messages about package compatibility between `dbt-autofix` and Fusion warnings.

Here's why:

* Fusion warnings are emitted based on a package's `require-dbt-version` and whether `require-dbt-version` contains `2.0.0`.
* Some packages are already Fusion-compatible even though package maintainers haven't yet updated `require-dbt-version`.
* `dbt-autofix` knows about these compatible packages and will not try to upgrade a package that it knows is already compatible.

This means that even if you see a Fusion warning for a package that `dbt-autofix` identifies as compatible, you don't need to change the package.

The message discrepancy is temporary while we implement and roll out `dbt-autofix`'s enhanced compatibility detection to Fusion warnings.

Here's an example of a Fusion warning in the Studio IDE that says a package isn't compatible with Fusion but `dbt-autofix` indicates it is compatible:

```text
dbt1065: Package 'dbt_utils' requires dbt version [>=1.30,<2.0.0], but current version is 2.0.0-preview.72. This package may not be compatible with your dbt version. dbt(1065) [Ln 1, Col 1]
```

### Step 1: Review your current packages[​](#step-1-review-your-current-packages "Direct link to Step 1: Review your current packages")

Identify which packages your project uses:

1. In the Studio IDE, open your project's root directory.
2. Look for either `packages.yml` or `dependencies.yml` file.
3. Review the list of packages and their current versions.

Your file will look something like this:

```yaml
packages:
  - package: dbt-labs/dbt_utils
    version: 1.0.0
  - package: dbt-labs/codegen
    version: 0.9.0
```

### Step 2: Check compatibility and find the latest package versions[​](#step-2-check-compatibility-and-find-the-latest-package-versions "Direct link to Step 2: Check compatibility and find the latest package versions")

Review [the dbt package hub](https://hub.getdbt.com) to see verified Fusion-compatible packages by checking that the `require-dbt-version` configuration includes `2.0.0` or higher. Refer to [package support](https://docs.getdbt.com/docs/fusion/supported-features.md#package-support) for more information.

For packages that aren't Fusion-compatible:

* Visit the package's GitHub repository.
* Check the README or recent releases for Fusion compatibility information.
* Look for issues or discussions about Fusion support.

For each package, find the most recent version:

* Visit [dbt Hub](https://hub.getdbt.com) for packages hosted there.
* For packages from GitHub, check the repository's releases page.
* Note the latest version number for each package you use.

For Hub packages, you can use version ranges to stay up-to-date:

```yaml
packages:
  - package: dbt-labs/dbt_utils
    version: [">=1.0.0", "<3.0.0"]  # Gets latest 1.x or 2.x version
```

### Step 3: Update your package versions[​](#step-3-update-your-package-versions "Direct link to Step 3: Update your package versions")

Update your `packages.yml` or `dependencies.yml` file with the latest compatible versions:

1. In the Studio IDE, open your `packages.yml` or `dependencies.yml` file.

2. Update each package version to the latest compatible version.

3. Save the file.

   Before update:

   ```yaml
   packages:
   - package: dbt-labs/dbt_utils
      version: 0.9.6
   - package: dbt-labs/codegen
      version: 0.9.0
   ```

   After update:

   ```yaml
   packages:
   - package: dbt-labs/dbt_utils
      version: [">=1.0.0", "<2.0.0"]
   - package: dbt-labs/codegen
      version: [">=0.12.0", "<1.0.0"]
   ```

### Step 4: Install updated packages[​](#step-4-install-updated-packages "Direct link to Step 4: Install updated packages")

After updating your package versions, install them:

1. In the Studio IDE command line, run:

   <!-- -->

   ```bash
   dbt deps --upgrade
   ```

The `--upgrade` flag ensures dbt installs the latest versions within your specified ranges, updating the `package-lock.yml` file.

2. Review the output to confirm all packages installed successfully.
3. Check that the `package-lock.yml` file was updated with the new package versions.

About package-lock.yml

The `package-lock.yml` file pins your packages to specific versions for reproducible builds. We recommend committing this file to version control so your entire team uses the same package versions.

### Step 5: Test your project with updated packages[​](#step-5-test-your-project-with-updated-packages "Direct link to Step 5: Test your project with updated packages")

After upgrading packages, test your project to ensure everything works:

1. Run a subset of your models to verify basic functionality:

   ```bash
   dbt run --select tag:daily
   ```

2. Run your tests to catch any breaking changes (exact command may vary):

   ```bash
   dbt test
   ```

3. If you encounter issues:

   * Review the package's changelog for breaking changes
   * Adjust your code to match new package behavior
   * If problems persist, temporarily pin to an older compatible version (if possible)

### Step 6: Commit package updates[​](#step-6-commit-package-updates "Direct link to Step 6: Commit package updates")

Once you've verified the updated packages work correctly:

1. In the **Version control** panel, stage your changes:

   * `packages.yml` or `dependencies.yml`
   * `package-lock.yml`

2. Add a commit message like "Upgrade dbt packages for Fusion compatibility".

3. Click **Commit and sync**.

## Update your jobs[​](#update-your-jobs "Direct link to Update your jobs")

Use the Fusion readiness panel to validate your jobs against the dbt Fusion engine and fix any issues you find.

For jobs that are eligible for Fusion in the readiness experience, **Run once on Fusion** is replaced by a **Debug on Fusion** dropdown in the eligibility banner or modal. Use a debug option when you want to inspect or fix issues interactively in Studio IDE. Use **Run once on Fusion** when you want to validate execution behavior in a deployment context.

### Review your jobs[​](#review-your-jobs "Direct link to Review your jobs")

The readiness panel shows how many jobs are ineligible for Fusion or have an unknown eligibility status. If you don't see eligibility messaging, ask an account admin to enable **Fusion readiness & upgrade features** in [Account settings](https://docs.getdbt.com/docs/platform/account-settings.md). For setup steps, refer to [Enable Fusion readiness features](https://docs.getdbt.com/guides/prepare-fusion-upgrade.md?step=3).

note

If a job has not run in the last 7 days, you must run it once for the debugging options to be available.

1. Open the jobs list using either path:

   <!-- -->

   * From the main menu, go to **Orchestration** → **Jobs**.
   * From the readiness panel, click **Review jobs**.

   [![Shortcut to review your jobs from the readiness panel](/img/fusion/review-jobs.png?v=2 "Shortcut to review your jobs from the readiness panel")](#)Shortcut to review your jobs from the readiness panel

2. Find the Fusion eligibility icon to the right of your jobs. Click **Review job** for any job that is ineligible or has an unknown eligibility status.
   <!-- -->
   [![Take action on your jobs to make them Fusion eligible.](/img/fusion/job-eligibility.png?v=2 "Take action on your jobs to make them Fusion eligible.")](#)Take action on your jobs to make them Fusion eligible.

3. Click **Debug on Fusion** and choose one of the following:

   <!-- -->

   * [Debug in Studio](#debug-in-studio)
   * [Debug in Studio with dbt Wizard](#debug-in-studio-with-dbt-wizard)
   * [Run once on Fusion](#run-once-on-fusion)

#### Debug in Studio[​](#debug-in-studio "Direct link to Debug in Studio")

In the Studio IDE, run Fusion in your development environment to review project warnings and errors:

1. Click **Debug in Studio**. dbt sets your user-level `DBT_DEVELOP_CORE_VERSION` environment variable to `latest-fusion`, then opens the Studio IDE with the **Problems** tab selected.

[![Running Fusion in development](/img/fusion/fusion-ide.png?v=2 "Running Fusion in development")](#)Running Fusion in development

2. Review the warnings or errors in the **Problems** tab.
3. Fix the issues directly or run the [autofix tool](https://docs.getdbt.com/docs/platform/studio-ide/autofix-deprecations.md).
4. When the project runs with no warnings or errors, commit and publish your changes.
5. After you merge the changes, wait for the job to run again or run it manually.

To revert the `latest-fusion` override, use the dbt version control in Studio IDE or update the **dbt version** under **User development settings** in [Account settings](https://docs.getdbt.com/docs/platform/account-settings.md) → **Credentials**. For more details, refer to [Override dbt version](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-platform-version.md#override-dbt-version).

#### Debug in Studio with dbt Wizard [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[​](#debug-in-studio-with-dbt-wizard- "Direct link to debug-in-studio-with-dbt-wizard-")

If you have access to [dbt Wizard](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md) with [AI features](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md?version=2.0#enable-dbt-wizard) enabled, you can use the [Fusion migration workflow](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md#fusion-migration-workflow) skill. This skill can help you fix compatibility errors directly from the Studio IDE using dbt Wizard — no manual log investigation needed. It classifies every error, applies validated fixes automatically, and surfaces what's blocked.

info

The Fusion migration workflow is accessible through the dbt Wizard in the Studio IDE. If you're using VS Code or the dbt CLI, use the [autofix tool](https://docs.getdbt.com/guides/fusion-package-compat?step=4) instead.

1. From the job list, click the **Review job** button for a job with a successful run.
   <!-- -->
   * If you don't see the **Review job** button, enable the **Show Fusion eligibility** toggle in the job list.

2. In the **Fusion eligibility unknown for this job** pop-up, click **Debug in Studio with dbt Wizard**.

3. dbt redirects you to the Studio IDE and sets your personal development environment to Fusion.

4. dbt Wizard opens and automatically triggers the Fusion migration skill with this prompt:

   <!-- -->

   ```text
   I need help fixing Fusion compatibility issues in this project. Please investigate and resolve any deprecation warnings or incompatibilities. Please use the migrating-dbt-core-to-fusion skill to guide this.
   ```

5. Review and approve dbt Wizard's permission requests so it can run the commands it needs.

6. The dbt Wizard iteratively runs `dbt compile`, reads the results, and applies fixes until it reaches a successful compile or encounters an error it can't resolve. If it gets blocked, it exits cleanly, explains what it could not fix, and creates and links to a markdown file summarizing all changes made.

7. When the project compiles with no warnings or errors, commit and publish your changes.

8. After you merge the changes, wait for the job to run again or run it manually on Fusion.

[![The Developer Agent's fusion migration workflow triaging and fixing Fusion compatibility errors in the Studio IDE.](/img/docs/dbt-platform/fusion-migration-workflow.gif?v=2 "The Developer Agent's fusion migration workflow triaging and fixing Fusion compatibility errors in the Studio IDE.")](#)The Developer Agent's fusion migration workflow triaging and fixing Fusion compatibility errors in the Studio IDE.

#### Run once on Fusion[​](#run-once-on-fusion "Direct link to Run once on Fusion")

When you are confident a job is ready for Fusion, you can run it once on the engine without changing environment-wide settings. **Run once on Fusion** can be temporarily unavailable while a Fusion run request is already pending.

1. Click **Run once on Fusion**.
2. The job window opens and dbt runs the job on Fusion without changing other jobs or environment settings.
3. When the job succeeds, click **Override eligibility status** to update the eligibility status.
   <!-- -->
   [![Override the eligibility status of a successful job.](/img/fusion/eligibility-status.png?v=2 "Override the eligibility status of a successful job.")](#)Override the eligibility status of a successful job.

Congratulations! You have validated Fusion eligibility for your jobs.

[![Your job is now ready for Fusion!](/img/fusion/fusion-eligible.png?v=2 "Your job is now ready for Fusion!")](#)Your job is now ready for Fusion!

## What's next?[​](#whats-next "Direct link to What's next?")

With limitations identified and addressed, you've completed all the preparation steps. Your project is now ready to upgrade to Fusion!

Check out [Part 2: Making the move](https://docs.getdbt.com/guides/upgrade-to-fusion.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
