# Install dbt Wizard CLI [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Local development

Install the dbt Wizard CLI from your terminal for agentic and governed data development in dbt.

This guide explains how to install, verify, update, and uninstall the dbt Wizard CLI on your local machine.

Wizard usage and billing

From September 1st, 2026, dbt Wizard usage is metered per token against your account's usage credits. All credit amounts are per account, not per user. Enterprise and Enterprise+ accounts get monthly credits. Developer and Starter plans start with a 30-day trial and $100 in credits, as do CLI users via a free dbt account.

Refer to [Trial and billing](./pricing-billing/trial-and-billing.md) for what each plan gets, spend limits, and paid access.

You can run the dbt Wizard CLI locally from any project on any dbt engine. Be warned, the wizard has been known to cast spells

.

## Prerequisites

* macOS, Windows, or Linux
* A dbt project with a built `target/` directory (`dbt parse`, `dbt compile`, or `dbt build`)
* Access to a supported AI provider. You can use a managed provider in dbt, or configure [BYOK](./wizard-byok.md) with your own provider credentials.

### Supported AI models

dbt Wizard supports [managed models](./pricing-billing/overview.md#dbt-managed-providers) (billed by dbt Labs, no key to manage) and [bring-your-own-key (BYOK)](./wizard-byok.md) models (billed directly by your provider).

Here are the following AI providers supported depending on where you work. Refer to [Model Provider Rate table](https://www.getdbt.com/legal/dbt-wizard-token-costs-by-model) for the full list of available models.

#### dbt platform

| Provider                                                                     | Access              |
| ---------------------------------------------------------------------------- | ------------------- |
| [OpenAI](https://openai.com/policies/row-terms-of-use/) (default)            | dbt managed or BYOK |
| [Anthropic](https://www.anthropic.com/legal/consumer-terms)†                 | dbt managed or BYOK |
| Open weight models (like DeepSeek, Kimi, and so on).                         | dbt managed         |
| [Azure AI Foundry](https://www.microsoft.com/licensing/terms) / Azure OpenAI | BYOK                |

#### Locally (CLI)

| Provider                                                                     | Access              |
| ---------------------------------------------------------------------------- | ------------------- |
| [OpenAI](https://openai.com/policies/row-terms-of-use/)                      | dbt managed or BYOK |
| [Anthropic](https://www.anthropic.com/legal/consumer-terms)†                 | dbt managed or BYOK |
| Open weight models (like DeepSeek, Kimi, and so on).                         | dbt managed         |
| [Azure AI Foundry](https://www.microsoft.com/licensing/terms) / Azure OpenAI | BYOK                |
| [AWS Bedrock](https://aws.amazon.com/service-terms/)                         | BYOK                |
| [Google Gemini](https://ai.google.dev/gemini-api/terms)                      | BYOK                |
| [Snowflake Cortex](https://www.snowflake.com/en/legal/terms-of-service/)     | BYOK                |
| [Databricks Unity AI Gateway](https://www.databricks.com/legal/mcsa)         | BYOK                |

You can also connect a personal OpenAI ChatGPT subscription instead of a key.

†Anthropic enterprise and subscription licenses (such as Claude Enterprise) aren't supported per Anthropic's [terms of service](https://www.anthropic.com/legal/consumer-terms).

New to the terminal?

If you've never used the terminal before, check out the [terminal guide](../../guides/terminal-guide.md) for for some helpful tips to help you get started!

## Install and set up dbt Wizard CLI

(Applies to dbt v1.99 and earlier)

Upgrade for automatic updates

Upgrade to [v2](../dbt-versions/dbt-upgrade/upgrading-to-v2.md) to run dbt Wizard as `wizard` and get automatic updates.

12345

View all stepsNext

1

Install the dbt Wizard CLI

Run the install script for your operating system:

macOS/Linux:

```bash
curl -fsSL https://public.cdn.getdbt.com/dbt-wizard/install/install-wizard.sh | sh
```

Windows (PowerShell):

```powershell
irm https://public.cdn.getdbt.com/dbt-wizard/install/install-wizard.ps1 | iex
```

This installs dbt Wizard to `/usr/local/bin/wizard`, along with the dbt [metadata engine](./wizard-how-it-works.md#native-metadata-engine) that powers dbt Wizard's project-aware answers. For install and update details, refer to [Install dbt Wizard CLI](./wizard-cli.md); to remove them, refer to [Uninstall](./wizard-cli.md#uninstall).

## Update

Run the following command to update dbt Wizard to the latest version:

```bash
wizard update
```

## Uninstall

1. Run the built-in uninstall command. It lists every binary, config, and data directory it's about to remove, then asks you to confirm (`Proceed? [Y/N]`) before deleting anything:

   ```shell
   wizard system uninstall
   ```

Uninstalling Wizard

Removing `~/.dbt/wizard` deletes your local config, logs, and cache, and can't be undone. Your dbt profiles (`~/.dbt/`) and dbt projects aren't part of dbt Wizard and won't be touched.

2. Confirm the binary is deleted by checking your system path:

   ```bash
   which wizard
   ```

If no output path is returned, dbt Wizard is successfully uninstalled.

## Telemetry

dbt Wizard collects anonymous product telemetry to improve the AI agent experience, understand usage patterns, optimize performance, and attribute compute costs without capturing your code, queries, prompts, responses, or file contents.

For details about what is collected, what is not collected, and how to opt out of client telemetry, refer to [dbt Wizard CLI data use and telemetry](./wizard-telemetry.md).

Best practices for using dbt Wizard

Once you're set up, refer to [How to use dbt Wizard in your dbt project](../../best-practices/how-to-use-wizard/wizard-1-intro.md) for recommended workflows on real project tasks.

## Related docs

* [Use dbt Wizard locally](./wizard-quickstart.md): Install dbt Wizard and start a local terminal session
* [Configure BYOK](./wizard-byok.md): Manage your API key and choose an AI model
* [Command reference](./wizard-cli-reference.md): Full reference for all `wizard` subcommands and global flags
* [Use cases and examples](./wizard-use-cases.md): Realistic analytics engineering scenarios
* [Migrate from another AI agent](./wizard-migrate.md): Migrate from another AI agent to dbt Wizard
* [CLI data use and telemetry](./wizard-telemetry.md): What dbt Wizard CLI collects and how to opt out
* [How to use dbt Wizard in your dbt project](../../best-practices/how-to-use-wizard/wizard-1-intro.md) for recommended workflows

See it in action and share your feedback

Want to see dbt Wizard in action? Check out the [demo video](https://www.youtube.com/watch?v=-lIzh1xQWMA).

We'd love to hear how dbt Wizard is working for you. Share your feedback by either running the `/feedback` slash command in your interactive terminal session or by going to the [#dbt-wizard](https://getdbt.slack.com/archives/C0B6KLW6T26) channel in the [dbt Community Slack](https://docs.getdbt.com/community/join?version=2.0).

Thanks so much for your help in improving dbt Wizard and dbt data development!
