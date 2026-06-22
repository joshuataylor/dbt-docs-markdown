# Upgrade to Fusion [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

After [installing the dbt VS Code extension](https://docs.getdbt.com/docs/install-dbt-extension.md), use the **Get started** panel to upgrade your project to the dbt Fusion engine if you haven't already done so.

note

If you are already running the dbt Fusion engine, you must be on version `2.0.0-beta.66` or higher to use the upgrade tool.

The dbt extension provides two ways to upgrade your project to Fusion from the **Get started** panel:

* [**Agentic migration**:](#agentic-migration) Runs the dbt Core-to-Fusion migration with
  <!-- -->
  or AI agents. Select this option in the **Check Fusion compatibility** step. Requires
  <!-- -->
  or AI agents.
* [**Manual CLI onboarding**:](#manual-cli-onboarding) Walks you through the upgrade in your terminal. Use this if you prefer the CLI or don't use
  <!-- -->
  or AI agents.

## Agentic migration[​](#agentic-migration "Direct link to Agentic migration")

1. Select **Start with an agent** from the **Check Fusion compatibility** step.
2. The extension installs the **Migrate dbt Core to Fusion** agent skill to your editor's skill folder, then opens your AI chat with a migration prompt.
3. Click **Install & open chat** to continue. The chat opens with the migration prompt already loaded, so you can run it directly from the editor. No CLI commands are required.

## Manual CLI onboarding[​](#manual-cli-onboarding "Direct link to Manual CLI onboarding")

The **Get started** panel has an upgrade assistant that guides you through the upgrade process in your terminal.

[![The dbt extension Get started panel and upgrade assistant.](/img/docs/extension/vsce-manual-upgrade.png?v=2 "The dbt extension Get started panel and upgrade assistant.")](#)The dbt extension Get started panel and upgrade assistant.

You can start manual onboarding in either of the following ways:

* From the Get started panel
* From your terminal

- From the **Get started** panel, select **Start manually in CLI** in the **Check Fusion compatibility** step.
- Follow the prompts in the upgrade assistant to complete the upgrade.
- Run `dbtf compile` to verify your project is ready for Fusion.

[![The message received when you have completed upgrading your project to the dbt Fusion engine.](/img/docs/extension/fusion-onboarding-complete.png?v=2 "The message received when you have completed upgrading your project to the dbt Fusion engine.")](#)The message received when you have completed upgrading your project to the dbt Fusion engine.

Once the upgrade is completed, you're ready to dive into all the features that the dbt Fusion engine has to offer!

Next, [sign in or register](https://docs.getdbt.com/docs/sign-in-dbt-extension.md) for a dbt platform account to keep using advanced features after the 14-day trial.

* From your terminal, run:

  ```shell
  dbt init --fusion-upgrade
  ```

The upgrade tool guides you through a series of prompts:

 Do you have an existing dbt platform account?

If you answer `Y`, the tool provides instructions for downloading your dbt platform profile or authenticating. If you answer `N`, the flow continues to local project setup.

 Ready to run dbt init?

If no `profiles.yml` file is present, the tool guides you through creating one and connecting to your data warehouse.

 Ready to run dbt debug?

If a `profiles.yml` file exists, the tool validates that your project is configured correctly and can connect to your data warehouse.

 Ready to run dbt parse?

The tool parses your dbt project to check compatibility with Fusion.

* If parsing fails, the tool can run [dbt-autofix](https://github.com/dbt-labs/dbt-autofix?tab=readme-ov-file#installation) to help resolve errors.
* If you skip dbt-autofix, you can run it later or manually fix errors. The upgrade tool can't continue until parsing errors are resolved.

 Ready to run dbt compile with static analysis off?

After parsing succeeds, the tool compiles your project without static analysis. This mimics dbt Core behavior by rendering Jinja into SQL while temporarily disabling Fusion's advanced SQL comprehension.

AI Agents

There are cases where dbt-autofix may not resolve all errors and requires manual intervention. For those cases, the dbt-autofix tool provides an [AI Agents.md](https://github.com/dbt-labs/dbt-autofix/blob/main/AGENTS.md) file to help AI agents continue the migration work after dbt-autofix has completed its part.

 Ready to run dbt compile?

The tool compiles your project with full Fusion static analysis. This checks that your SQL code is valid in the context of your warehouse's tables and columns.

[![The message received when you have completed upgrading your project to the dbt Fusion engine.](/img/docs/extension/fusion-onboarding-complete.png?v=2 "The message received when you have completed upgrading your project to the dbt Fusion engine.")](#)The message received when you have completed upgrading your project to the dbt Fusion engine.

Once the upgrade is completed, you're ready to dive into all the features that the dbt Fusion engine has to offer!

Next, [sign in or register](https://docs.getdbt.com/docs/sign-in-dbt-extension.md) for a dbt platform account to keep using advanced features after the 14-day trial.

## Next steps[​](#next-steps "Direct link to Next steps")

Once you've upgraded your project to Fusion, you can:

* [Sign in or register](https://docs.getdbt.com/docs/sign-in-dbt-extension.md) for a dbt platform account to keep using advanced features after the 14-day trial.
* [Configure your local environment](https://docs.getdbt.com/docs/configure-dbt-extension.md) to mirror your dbt platform environment and [set environment variables](https://docs.getdbt.com/docs/configure-dbt-extension.md#configure-environment-variables) required by your project.
* [Compare changes locally](https://docs.getdbt.com/docs/fusion/vs-compare-changes.md) to preview data changes caused by your local edits.
* Review the [limitations and unsupported features](https://docs.getdbt.com/docs/fusion/supported-features.md#limitations).
