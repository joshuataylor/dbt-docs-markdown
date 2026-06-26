# Install dbt Wizard CLI [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Install the dbt Wizard CLI from your terminal for agentic and governed data development in dbt.

This guide explains how to install, verify, update, and uninstall the dbt Wizard CLI on your local machine. (Be warned, the wizard has been known to cast spells

)

You can run the dbt Wizard CLI locally from any dbt project that uses the dbt CLI, Fusion, or dbt Core.

Install dbt Wizard as `wizard` on your `PATH` using the curl script for your operating system:

* macOS/Linux
* Windows

```bash
curl -fsSL https://public.cdn.getdbt.com/dbt-wizard/install/install-wizard.sh | sh
```

Run the following in PowerShell:

```powershell
irm https://public.cdn.getdbt.com/dbt-wizard/install/install-wizard.ps1 | iex
```

Then verify the install and start a session:

```bash
wizard --version   # confirm the install
wizard             # start an interactive session
```

After running `wizard --version`, you should see something like `dbt-wizard VERSION`. Run `wizard --help` to see all available commands and flags. dbt Wizard installs default config files — refer to the [config reference](https://docs.getdbt.com/docs/dbt-ai/wizard-config.md) for more details.

<!-- -->

Next up, check out the [Prerequisites](#prerequisites) and [First-run setup and onboarding](#first-run-setup-and-onboarding) sections for more details.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* macOS, Windows, or Linux
* A dbt project with a built `target/` directory (`dbt parse`, `dbt compile`, or `dbt build`)
* Credentials for a supported CLI provider. Refer to [Supported AI providers](https://docs.getdbt.com/docs/dbt-ai/wizard-byok.md#supported-ai-providers) in the next section.

## Supported AI providers[​](#supported-ai-providers "Direct link to Supported AI providers")

#### dbt Wizard[​](#dbt-wizard "Direct link to dbt Wizard")

dbt Wizard supports different AI providers depending on where you use it.

| Provider                                                                     | dbt Wizard in dbt platform | dbt Wizard CLI                  |
| ---------------------------------------------------------------------------- | -------------------------- | ------------------------------- |
| [OpenAI](https://openai.com/policies/row-terms-of-use/)                      | ✓ (managed or BYOK)        | ✓ (OpenAI subscription or BYOK) |
| [Anthropic](https://www.anthropic.com/legal/consumer-terms)†                 | ✓ (BYOK)                   | ✓ (BYOK)                        |
| [Azure AI Foundry](https://www.microsoft.com/licensing/terms) / Azure OpenAI | ✓ (BYOK)                   | ✓ (BYOK)                        |
| [AWS Bedrock](https://aws.amazon.com/service-terms/)                         | -                          | ✓ (BYOK)                        |
| [Google Gemini](https://ai.google.dev/gemini-api/terms)                      | -                          | ✓ (BYOK)                        |
| [Snowflake Cortex](https://www.snowflake.com/en/legal/terms-of-service/)     | -                          | ✓ (BYOK)                        |
| [Databricks Unity AI Gateway](https://www.databricks.com/legal/mcsa)         | -                          | ✓ (BYOK)                        |

† *Anthropic enterprise and subscription licenses (such as Claude Enterprise) aren't supported per Anthropic's [terms of service](https://www.anthropic.com/legal/consumer-terms). BYOK requires an Anthropic API key.*

Refer to the following pages for more information:

* [Configure dbt platform](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md#enable-ai-features) integrations in account settings.
* [Configure BYOK for the CLI](https://docs.getdbt.com/docs/dbt-ai/wizard-byok.md) by running `wizard providers configure PROVIDER_NAME` and follow the prompts.

New to the terminal?

If you've never used the terminal before, check out the [terminal guide](https://docs.getdbt.com/guides/terminal-guide.md). Some tips include:

* Enter `/` to see the available commands or try out `/overview` to get a quick summary of your project.
* Press `Shift+Tab` to cycle through collaboration modes.

## First-run setup and onboarding[​](#first-run-setup-and-onboarding "Direct link to First-run setup and onboarding")

The first time you start dbt Wizard in a project, it walks you through a short setup and saves your answers to [`wizard_config.toml`, `providers.json`, and `provider-auth.json`](https://docs.getdbt.com/docs/dbt-ai/wizard-config.md), so you only do this once per project. You'll be asked to:

* Review and accept the **Terms of Use**.
* If you have a dbt platform account, sign in through the browser authentication link when prompted and follow the steps in the browser.
* **Trust the directory** so dbt Wizard can read your project.
* Confirm the **path to your dbt executable or virtual environment** — point it at `/path/to/bin/dbt` or a `.venv` root (dbt Wizard uses `bin/dbt` automatically).
* Add any **extra compile flags** to append to the startup `dbt compile -s state:modified+`, or leave empty to skip.
* **Configure [deferral](https://docs.getdbt.com/docs/dbt-ai/wizard-config.md#deferral)** — choose **Wizard** (dbt Wizard manages it), **Manual**, or **Disabled**. If you choose **Wizard**, enter the `profiles.yml` target to defer to (defaults to `prod`). On the dbt Fusion engine connected to the dbt platform, dbt Wizard instead offers to let the platform handle deferral.
* Confirm your **detected dbt profile and target**, or customize the profile, target, or `profiles.yml` path.
* **Configure a provider** (OpenAI subscription, OpenAI API key, Anthropic, Amazon Bedrock, Azure, Gemini, or Snowflake). Refer to [Configure BYOK](https://docs.getdbt.com/docs/dbt-ai/wizard-byok.md).

To re-run any of these steps later, refer to [Re-trigger onboarding flows](https://docs.getdbt.com/docs/dbt-ai/wizard-config.md#re-trigger-onboarding-flows).

Set your API key without the prompt

During onboarding, dbt Wizard prompts you to configure a provider interactively. To skip the API key prompt — for headless runs like `wizard exec` or to reuse your key across sessions — set it as an environment variable before starting `wizard` instead:

```bash
export OPENAI_API_KEY="sk-..."                  # or ANTHROPIC_API_KEY
export AWS_BEARER_TOKEN_BEDROCK="ABSK..."       # for Amazon Bedrock
```

For AWS Bedrock and Snowflake Cortex, refer to [Configure BYOK](https://docs.getdbt.com/docs/dbt-ai/wizard-byok.md).

After onboarding, dbt Wizard shows a welcome screen with two sections:

* **STATUS** — the dbt Wizard version, the active AI model (change it with `/model`), and your project directory.
* **OVERVIEW** — a snapshot of your project from the [metadata engine](https://docs.getdbt.com/docs/dbt-ai/wizard-how-it-works.md#native-metadata-engine): build status and a count of passing (✓), warning (⚠), and failing (✗) checks.

Enter `/` to see the available [slash commands](https://docs.getdbt.com/docs/dbt-ai/wizard-slash-commands.md), or try `/overview` for a summary of your project. CLI commands use the `wizard` prefix, so you can also run [subcommands](https://docs.getdbt.com/docs/dbt-ai/wizard-cli-reference.md) such as `wizard exec`, `wizard review`, and `wizard resume`.

## Update[​](#update "Direct link to Update")

Run the following command to update dbt Wizard to the latest version:

```bash
wizard update
```

## Telemetry[​](#telemetry "Direct link to Telemetry")

dbt Wizard collects anonymous product telemetry to improve the AI agent experience, understand usage patterns, optimize performance, and attribute compute costs without capturing your code, queries, prompts, responses, or file contents.

For details about what is collected, what is not collected, and how to opt out of client telemetry, refer to [dbt Wizard CLI data use and telemetry](https://docs.getdbt.com/docs/dbt-ai/wizard-telemetry.md).

## Related docs[​](#related-docs "Direct link to Related docs")

* [Get started with the local CLI](https://docs.getdbt.com/docs/dbt-ai/wizard-quickstart.md): Install dbt Wizard and start a local terminal session
* [Configure BYOK](https://docs.getdbt.com/docs/dbt-ai/wizard-byok.md): Manage your API key and choose an AI model
* [Command reference](https://docs.getdbt.com/docs/dbt-ai/wizard-cli-reference.md): Full reference for all `wizard` subcommands and global flags
* [Use cases and examples](https://docs.getdbt.com/docs/dbt-ai/wizard-use-cases.md): Realistic analytics engineering scenarios
* [Migrate from another AI agent](https://docs.getdbt.com/docs/dbt-ai/wizard-migrate.md): Migrate from another AI agent to dbt Wizard
* [CLI data use and telemetry](https://docs.getdbt.com/docs/dbt-ai/wizard-telemetry.md): What dbt Wizard CLI collects and how to opt out

See it in action and share your feedback

Want to see dbt Wizard in action? Check out the [demo video](https://www.youtube.com/watch?v=-lIzh1xQWMA).

We'd love to hear how dbt Wizard is working for you. Share your feedback by either running the `/feedback` slash command in your interactive terminal session or by going to the [#dbt-wizard](https://getdbt.slack.com/archives/C0B6KLW6T26) channel in the [dbt Community Slack](https://docs.getdbt.com/community/join?version=2.0).

Thanks so much for your help in improving dbt Wizard and dbt data development!
