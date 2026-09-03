# About dbt Wizard

Local development | dbt platform

Your personal dbt agent — wherever you work.

dbt Wizard is an AI agent purpose-built for governed data development in dbt. Unlike general-purpose coding agents, it understands your dbt project through a [native metadata engine](../dbt-ai/wizard-how-it-works.md#native-metadata-engine) — a structured index of lineage, model health, tests, contracts, run results, and semantic definitions.

Think of it like a map of your city: dbt Wizard knows how everything connects before it starts, rather than walking every street to figure out the layout. dbt Wizard comes with:

* **Project understanding:** A native dbt metadata engine for lineage, contracts, tests, and runtime context
* **Impact awareness:** Checks upstream and downstream dependencies before you change code
* **Safe validation:** Compiles and builds changes before review
* **Complete workflow:** Investigate, change, validate, and review in one place
* **Setup and governance:** Works out of the box with dbt governance built in
* **Conversational analytics:** Answers questions about production data in plain language through [Explore mode](../dbt-ai/wizard-ide.md#agent-modes), grounded in governed metric definitions

Wizard usage and billing

From September 1st, 2026, dbt Wizard usage is metered per token against your account's usage credits. All credit amounts are per account, not per user. Enterprise and Enterprise+ accounts get monthly credits. Developer and Starter plans start with a 30-day trial and $100 in credits, as do CLI users via a free dbt account.

Refer to [Trial and billing](../dbt-ai/pricing-billing/trial-and-billing.md) for what each plan gets, spend limits, and paid access.

## Use dbt Wizard

dbt Wizard is for anyone doing dbt development. You can use it in the platform with managed or bring-your-own-key (BYOK) credentials, or in the terminal with your own key, with or without a dbt platform account. dbt Wizard is data warehouse agnostic and works on any [dbt version](../introduction.md#dbt-versions).

It's also for people who don't build data at all. In the dbt platform, [Explore mode](../dbt-ai/wizard-ide.md#agent-modes) lets [read-only users](./wizard-read-only-users.md) ask questions of production data in plain language, with no developer license and nothing to set up.

The following table shows where dbt Wizard is available, the AI keys each surface uses, and how usage is billed:

| Where                                                                                     | Status         | AI access options                         |
| ----------------------------------------------------------------------------------------- | -------------- | ----------------------------------------- |
| [dbt platform: Studio IDE](../dbt-ai/wizard-ide.md)             | Public preview | dbt managed or BYOK                       |
| [dbt platform: dbt Wizard home tab](./wizard-home.md) | Public preview | dbt managed or BYOK                       |
| [Locally: Terminal (CLI)](../dbt-ai/wizard-cli.md)              | Public beta    | dbt managed, BYOK, or OpenAI subscription |

## Supported providers

dbt Wizard supports [managed models](../dbt-ai/pricing-billing/overview.md#dbt-managed-providers) (billed by dbt Labs, no key to manage) and [bring-your-own-key (BYOK)](../dbt-ai/wizard-byok.md) models (billed directly by your provider).

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

For pricing and how billing works, refer to [Models and pricing](../dbt-ai/pricing-billing/overview.md).

## Get started

You can get started with Wizard wherever you work, whether it's the terminal or the dbt platform:

* [Wizard in the CLI](#wizard-in-the-cli)
* [Wizard in dbt platform](#wizard-in-dbt-platform)

(Be warned, the wizard has been known to cast spells

.)

### Wizard in the CLI [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

A terminal-based agent for governed data development in dbt, whether your team uses the dbt platform or self-hosts. Bring your own key to experience the full agentic analytics engineering loop.

(Applies to dbt v1.99 and earlier)

Upgrade for automatic updates

Upgrade to [v2](../dbt-versions/dbt-upgrade/upgrading-to-v2.md) to run dbt Wizard as `wizard` and get automatic updates.

12

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

This installs dbt Wizard to `/usr/local/bin/wizard`, along with the dbt [metadata engine](../dbt-ai/wizard-how-it-works.md#native-metadata-engine) that powers dbt Wizard's project-aware answers.

For first-run setup and billing, refer to [Use dbt Wizard locally](../dbt-ai/wizard-quickstart.md).

New to the terminal?

If you've never used the terminal before, check out the [terminal guide](../../guides/terminal-guide.md) for for some helpful tips to help you get started!

### Wizard in dbt platform [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Leverage agentic capabilities in the home app or [Studio IDE](../dbt-ai/wizard-ide.md) for governed data development in dbt.

AI features are on by default, so most accounts can jump straight in. If your account has them turned off, an admin can turn them back on in [Account settings](./manage-dbt-ai.md#manage-ai-features).

To get started:

1. Sign in to the [dbt platform](https://www.getdbt.com/signup), or create a free account if you don't have one yet.

2. Open dbt Wizard from the **home tab** in the left sidebar, or from [Studio IDE](../dbt-ai/wizard-ide.md) to work alongside the file editor.

3. Start with your usage credits or a 30-day trial. Enterprise and Enterprise+ accounts get monthly usage credits; all other plans can start a trial. Refer to [Trial and billing](../dbt-ai/pricing-billing/trial-and-billing.md).

4. Try a prompt, such as:

   * `summarize what this project does`
   * `list all models with no tests`
   * `add not_null and unique tests to the primary key of stg_customers`

Refer to [Use cases and examples](../dbt-ai/wizard-use-cases.md) for more prompts.

## Related docs

* [dbt Wizard in Studio IDE](../dbt-ai/wizard-ide.md) — generate docs, tests, semantic models, SQL, and delegate end-to-end model work
* [Invite read-only users to dbt Wizard](./wizard-read-only-users.md) — let business users ask questions of production data in Explore mode, without development access
* [Use skills in the dbt platform](../dbt-ai/wizard-platform-skills.md) — give dbt Wizard reusable instructions for your project
* [Use MCP servers with dbt Wizard CLI](../dbt-ai/wizard-mcp.md) — connect the dbt Wizard CLI to more tools and context
* [Migrate to dbt Wizard](../dbt-ai/wizard-migrate.md) — switch from Claude Code, Cursor, or another AI agent to dbt Wizard
* [Privacy and data FAQs](../dbt-ai/dbt-ai-faqs.md#privacy-and-data) — understand how dbt Wizard handles privacy and data

See it in action and share your feedback

Want to see dbt Wizard in action? Check out the [demo video](https://www.youtube.com/watch?v=-lIzh1xQWMA).

We'd love to hear how dbt Wizard is working for you. Share your feedback by either running the `/feedback` slash command in your interactive terminal session or by going to the [#dbt-wizard](https://getdbt.slack.com/archives/C0B6KLW6T26) channel in the [dbt Community Slack](https://docs.getdbt.com/community/join?version=2.0).

Thanks so much for your help in improving dbt Wizard and dbt data development!
