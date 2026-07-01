# About dbt Wizard in the dbt platform [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[Starter](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

dbt Wizard helps teams ship trusted dbt changes faster and with less risk. It uses native dbt metadata, routes to the right tools, and validates with warehouse awareness so teams can investigate, change, validate, and ship in one place.

See it in action and share your feedback

Want to see dbt Wizard in action? Check out the [demo video](https://www.youtube.com/watch?v=-lIzh1xQWMA).

We'd love to hear how dbt Wizard is working for you. Share your feedback by either running the `/feedback` slash command in your interactive terminal session or by going to the [#dbt-wizard](https://getdbt.slack.com/archives/C0B6KLW6T26) channel in the [dbt Community Slack](https://docs.getdbt.com/community/join?version=2.0).

Thanks so much for your help in improving dbt Wizard and dbt data development!

dbt Wizard is more than a general coding agent with access to dbt. Built for governed data development in dbt, it understands lineage, documentation, tests, and semantic definitions, and accounts for dev builds, compute, run time, and post-build inspection. Its suggestions are grounded in your project's actual data *and* context.

In [approval mode](https://docs.getdbt.com/docs/dbt-ai/wizard-how-it-works.md#approval-and-review), dbt Wizard shows every file change as a diff before anything is persisted. Built-in [dbt Agent Skills](https://github.com/dbt-labs/dbt-agent-skills) encode dbt best practices for consistent output. An admin must [enable dbt Wizard](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md) for your account before you can use it in the platform.

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

Looking for the terminal experience? Visit [About dbt Wizard CLI](https://docs.getdbt.com/docs/dbt-ai/about-dbt-wizard-cli.md).

## What you can do[​](#what-you-can-do "Direct link to What you can do")

Use dbt Wizard in the dbt platform to:

* Leverage agentic capabilities in the [Wizard home tab](https://docs.getdbt.com/docs/platform/wizard-home.md) or [Studio IDE](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md) for governed data development in dbt.
* Ask project-aware questions and get context-grounded answers
* Generate documentation, semantic models, tests, and metrics
* Build or refactor models from plain-language prompts
* Review file changes as diffs before persisting
* Run end-to-end tasks in approval or automatic edit modes

For more examples, visit [Use cases and examples](https://docs.getdbt.com/docs/dbt-ai/wizard-use-cases.md).

tip

Always review AI-generated content, as it may be incorrect. For prompt best practices, refer to the [Prompt cookbook](https://docs.getdbt.com/guides/prompt-cookbook.md).

*dbt Wizard is available in Studio IDE as a public preview feature, and as a standalone beta feature across the managed dbt platform. However, certain customers may have disabled experimental features, in which case, they can use Wizard CLI via terminal-access and will continue to have access to dbt Copilot until dbt Wizard is released as generally available to all customers. [Contact dbt Support](mailto:support@getdbt.com) with any questions.*

Please contact dbt Support with any questions

## Get started in the dbt platform[​](#get-started-in-the-dbt-platform "Direct link to Get started in the dbt platform")

[![](/img/icons/wizard.svg)](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md)

#### [Get started in dbt platform](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md)

[Turn on dbt Wizard and dbt Copilot for your account in the dbt platform.](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md)

[![](/img/icons/wizard.svg)](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md)

#### [Wizard in Studio IDE](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md)

[Leverage agentic development in the Studio IDE for governed data development in dbt.](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md)

[![](/img/icons/wizard.svg)](https://docs.getdbt.com/docs/platform/wizard-home.md)

#### [Wizard home tab](https://docs.getdbt.com/docs/platform/wizard-home.md)

[Use the agent-native home tab to iterate in natural language, review inline diffs, and validate changes.](https://docs.getdbt.com/docs/platform/wizard-home.md)

[![](/img/icons/wizard.svg)](https://docs.getdbt.com/docs/dbt-ai/wizard-platform-skills.md)

#### [Use skills](https://docs.getdbt.com/docs/dbt-ai/wizard-platform-skills.md)

[Give dbt Wizard reusable instructions for your project.](https://docs.getdbt.com/docs/dbt-ai/wizard-platform-skills.md)

[![](/img/icons/wizard.svg)](https://docs.getdbt.com/docs/dbt-ai/wizard-platform-mcp.md)

#### [Use MCP servers](https://docs.getdbt.com/docs/dbt-ai/wizard-platform-mcp.md)

[Understand MCP server support in the dbt platform experience.](https://docs.getdbt.com/docs/dbt-ai/wizard-platform-mcp.md)

[![](/img/icons/wizard.svg)](https://docs.getdbt.com/docs/dbt-ai/wizard-platform-privacy-data.md)

#### [Data & Privacy](https://docs.getdbt.com/docs/dbt-ai/wizard-platform-privacy-data.md)

[Understand how dbt Wizard in the dbt platform handles privacy and data.](https://docs.getdbt.com/docs/dbt-ai/wizard-platform-privacy-data.md)
