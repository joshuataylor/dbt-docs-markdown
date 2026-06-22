# About dbt Wizard

Your personal dbt agent — wherever you work.

dbt Wizard is an AI agent purpose-built for governed data development in dbt. Unlike general-purpose coding agents, it understands your dbt project through a [native metadata engine](https://docs.getdbt.com/docs/dbt-ai/wizard-how-it-works.md#native-metadata-engine) — a structured index of lineage, model health, tests, contracts, run results, and semantic definitions.

Think of it like a map of your city: dbt Wizard knows how everything connects before it starts, rather than walking every street to figure out the layout.

dbt Wizard comes with the following capabilities:

* **Project understanding:** A native dbt metadata engine for lineage, contracts, tests, and runtime context
* **Impact awareness:** Checks upstream and downstream dependencies before you change code
* **Safe validation:** Compiles and builds changes before review
* **Complete workflow:** Investigate, change, validate, and review in one place
* **Setup and governance:** Works out of the box with dbt governance built in

## Use dbt Wizard[​](#use-dbt-wizard "Direct link to Use dbt Wizard")

dbt Wizard is for anyone doing dbt development — from analytics engineers working locally in the terminal to teams building in the dbt platform. You can use it in the platform with managed or bring-your-own-key (BYOK) credentials, or in the terminal with your own key, with or without a dbt platform account.

dbt Wizard is data warehouse agnostic and works with both the [dbt Fusion engine](https://docs.getdbt.com/docs/fusion.md) and [dbt Core](https://docs.getdbt.com/docs/local/install-dbt.md) — no specific engine is required.

The following table shows where dbt Wizard is available, the AI keys each surface uses, and how usage is billed:

| Where                                                                                      | Status         | AI provider keys             | Availability and cost                                                                                                                                                                                               |
| ------------------------------------------------------------------------------------------ | -------------- | ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [dbt platform — Studio IDE](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md)             | Public preview | Managed keys, or BYOK        | Managed usage is included with [dbt AI](https://docs.getdbt.com/docs/platform/billing.md#dbt-ai-usage-metering-and-limiting) by plan (not available on Developer). BYOK is available on Enterprise and Enterprise+. |
| [dbt platform — dbt Wizard home tab](https://docs.getdbt.com/docs/platform/wizard-home.md) | Public beta    | Managed keys, or BYOK        | Same as Studio IDE.                                                                                                                                                                                                 |
| [Terminal (CLI)](https://docs.getdbt.com/docs/dbt-ai/wizard-cli.md)                        | Public beta    | BYOK, or OpenAI subscription | You pay your AI provider directly. Works with or without a dbt platform account.                                                                                                                                    |

For included action limits by plan and how managed usage is metered, refer to the [Billing](https://docs.getdbt.com/docs/platform/billing.md) page. To bring your own key, refer to [supported providers](#supported-ai-providers) on this page.

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

## Get started with dbt Wizard[​](#get-started-with-dbt-wizard "Direct link to Get started with dbt Wizard")

* Terminal [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")
* dbt platform [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

*Available in public beta.*

A terminal-based agent for governed data development in dbt, whether your team uses the dbt platform or self-hosts. Bring your own key to experience the full agentic analytics engineering loop.

New to the terminal?

If you've never used the terminal before, check out the [terminal guide](https://docs.getdbt.com/guides/terminal-guide.md). Some tips include:

* Enter `/` to see the available commands or try out `/overview` to get a quick summary of your project.
* Press `Shift+Tab` to cycle through collaboration modes.

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

Upgrade for automatic updates

Upgrade to dbt Fusion engine or dbt Core v2.0 or later to run dbt Wizard as `wizard` and get automatic updates.

Your browser does not support the video tag.

dbt Wizard CLI in your terminal

*Available in public preview.*

Leverage agentic capabilities in the home app or [Studio IDE](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md) for governed data development in dbt.

To get started:

1. Make sure an admin has [enabled](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md) dbt Wizard.

2. Navigate to the dbt Wizard icon in the left sidebar of the dbt platform.

3. Open a project in [Studio IDE](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md) or the home app.

4. Try a prompt, such as:

   <!-- -->

   * `summarize what this project does`
   * `list all models with no tests`
   * `add not_null and unique tests to the primary key of stg_customers`

Refer to [Use cases and examples](https://docs.getdbt.com/docs/dbt-ai/wizard-use-cases.md) for more prompts.

[![dbt Wizard in the dbt platform](/img/docs/dbt-platform/wizard-home-agent.png?v=2 "dbt Wizard in the dbt platform")](#)dbt Wizard in the dbt platform

(Be warned, the wizard has been known to cast spells

.)

## Next steps[​](#next-steps "Direct link to Next steps")

Now that you know where to start, continue with **[Get started with the local CLI](https://docs.getdbt.com/docs/dbt-ai/wizard-quickstart.md)** for local installation and onboarding, or **[Get started in dbt platform](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md)** for the platform setup flow.

## Related docs[​](#related-docs "Direct link to Related docs")

* [dbt Wizard in Studio IDE](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md) — generate docs, tests, semantic models, SQL, and delegate end-to-end model work
* [Use skills in the dbt platform](https://docs.getdbt.com/docs/dbt-ai/wizard-platform-skills.md) — give dbt Wizard reusable instructions for your project
* [Use MCP servers with dbt Wizard CLI](https://docs.getdbt.com/docs/dbt-ai/wizard-mcp.md) — connect the dbt Wizard CLI to more tools and context
* [Use subagents in the dbt platform](https://docs.getdbt.com/docs/dbt-ai/wizard-platform-subagents.md) — delegate work to specialized agents
* [Migrate to dbt Wizard](https://docs.getdbt.com/docs/dbt-ai/wizard-migrate.md) — switch from Claude Code, Cursor, or another AI agent to dbt Wizard
* [Data & Privacy in the dbt platform](https://docs.getdbt.com/docs/dbt-ai/wizard-platform-privacy-data.md) — understand how dbt Wizard in the dbt platform handles privacy and data

See it in action and share your feedback

Want to see dbt Wizard in action? Check out the [demo video](https://www.youtube.com/watch?v=-lIzh1xQWMA).

We'd love to hear how dbt Wizard is working for you. Share your feedback by either running the `/feedback` slash command in your interactive terminal session or by going to the [#dbt-wizard](https://getdbt.slack.com/archives/C0B6KLW6T26) channel in the [dbt Community Slack](https://docs.getdbt.com/community/join?version=2.0).

Thanks so much for your help in improving dbt Wizard and dbt data development!

*dbt Wizard is available in Studio IDE as a public preview feature, and as a standalone beta feature across the managed dbt platform. However, certain customers may have disabled experimental features, in which case, they can use Wizard CLI via terminal-access and will continue to have access to dbt Copilot until dbt Wizard is released as generally available to all customers. [Contact dbt Support](mailto:support@getdbt.com) with any questions.*

Please contact dbt Support with any questions
