# Use dbt Wizard locally [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Local development

Install dbt Wizard locally and start an agentic dbt development session from your terminal.

You can run the dbt Wizard CLI locally from any project on any dbt engine. Be warned, the wizard has been known to cast spells

.

Wizard usage and billing

From September 1st, 2026, dbt Wizard usage is metered per token against your usage credits. Enterprise and Enterprise+ accounts get monthly credits. Developer and Starter plans start with a 30-day trial and $100 in credits, as do CLI users via a free dbt account.

Refer to [Trial and billing](./pricing-billing/trial-and-billing.md) for what each plan gets, spend limits, and paid access.

## Prerequisites

* Access to a [supported AI provider](#supported-ai-providers). Use a dbt managed provider or configure [BYOK](./wizard-byok.md) with your own provider credentials.
* A dbt project with a built `target/` directory (run `dbt parse`, `dbt compile`, or `dbt build`)

dbt Wizard is data warehouse agnostic and works with both the [dbt Fusion engine](../introduction.md) and [dbt Core](../local/install-dbt.md) — no specific engine is required.

## Supported AI providers

dbt Wizard supports [managed models](./pricing-billing/overview.md#dbt-managed-providers) (billed by dbt Labs, no key to manage) and [bring-your-own-key (BYOK)](./wizard-byok.md) models (billed directly by your provider).

Here are the following AI providers supported depending on where you work. Refer to [Model Provider Rate table](https://www.getdbt.com/legal/dbt-wizard-token-costs-by-model) for the full list of available models.

### dbt platform

| Provider                                                                     | Access              |
| ---------------------------------------------------------------------------- | ------------------- |
| [OpenAI](https://openai.com/policies/row-terms-of-use/) (default)            | dbt managed or BYOK |
| [Anthropic](https://www.anthropic.com/legal/consumer-terms)†                 | dbt managed or BYOK |
| Open weight models (like DeepSeek, Kimi, and so on).                         | dbt managed         |
| [Azure AI Foundry](https://www.microsoft.com/licensing/terms) / Azure OpenAI | BYOK                |

### Locally (CLI)

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

## Install and set up dbt Wizard

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

Best practices for using dbt Wizard

Once you're set up, refer to [How to use dbt Wizard in your dbt project](../../best-practices/how-to-use-wizard/wizard-1-intro.md) for recommended workflows on real project tasks.

## Useful terminal commands

Use the following commands to get started:

| Command                       | Description                                                                                                                     | Example                                       |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------- |
| `wizard "[prompt]"`           | Start an interactive session seeded with a prompt. Once you activate the session, you don't need to pass your prompt in quotes. | `wizard "summarize what this project does"`   |
| `wizard exec "[prompt]"`      | Run a single prompt non-interactively and exit                                                                                  | `wizard exec "list all models with no tests"` |
| `wizard review --uncommitted` | Non-interactive code review of uncommitted changes                                                                              | `wizard review --uncommitted`                 |
| `wizard review --base BRANCH` | Review diff against a base branch                                                                                               | `wizard review --base main`                   |
| `wizard resume`               | Resume a previous session                                                                                                       | `wizard resume --last`                        |
| `wizard apply`                | Apply the latest Wizard diff to your working directory                                                                          | `wizard apply TASK_ID`                        |
| `wizard login` / `logout`     | Authenticate with your dbt account                                                                                              | `wizard login`                                |
| `wizard mcp`                  | Manage MCP server connections                                                                                                   | `wizard mcp add dbt`                          |
| `wizard update`               | Update Wizard to the latest version                                                                                             | `wizard update`                               |

Need to re-run setup?

If you want to re-run onboarding — re-authenticate, reset project config, or retrigger the trusted folder prompt — refer to [Re-trigger onboarding flows](./wizard-config.md#re-trigger-onboarding-flows).

## Next steps

* [Use cases and examples](./wizard-use-cases.md) for realistic analytics engineering scenarios
* [Install and update reference](./wizard-cli.md) for full install, update, and uninstall details
* [Configure BYOK](./wizard-byok.md) for managing your API key and choosing an AI model
* [How to use dbt Wizard in your dbt project](../../best-practices/how-to-use-wizard/wizard-1-intro.md) for recommended workflows
* [Use skills locally](./wizard-skills.md) for giving Wizard reusable instructions for your project
* [Use MCP servers](./wizard-mcp.md) to connect dbt Wizard CLI to more tools and context
* [Migrate from Claude Code](./wizard-migrate.md) for bringing existing Claude Code project context into dbt Wizard

See it in action and share your feedback

Want to see dbt Wizard in action? Check out the [demo video](https://www.youtube.com/watch?v=-lIzh1xQWMA).

We'd love to hear how dbt Wizard is working for you. Share your feedback by either running the `/feedback` slash command in your interactive terminal session or by going to the [#dbt-wizard](https://getdbt.slack.com/archives/C0B6KLW6T26) channel in the [dbt Community Slack](https://docs.getdbt.com/community/join?version=2.0).

Thanks so much for your help in improving dbt Wizard and dbt data development!
