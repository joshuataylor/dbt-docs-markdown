# dbt Wizard CLI [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

The dbt Wizard CLI helps teams ship higher-quality dbt changes faster and with less risk. Built for governed data development in dbt, it understands your project, routes to the right dbt tools, validates changes, and shows how logic evolves from your local machine.

You can run the dbt Wizard CLI locally from any dbt project that uses the dbt CLI, Fusion, or dbt Core.

Install the dbt Wizard CLI by running the following commands:

* macOS/Linux
* Windows

Run the following in Terminal:

```bash
curl -fsSL https://public.cdn.getdbt.com/dbt-wizard/install/install-wizard.sh | sh
```

Run the following in your Windows command line tool:

```powershell
irm https://public.cdn.getdbt.com/dbt-wizard/install/install-wizard.ps1 | iex
```

For a first session walkthrough, visit [Use dbt Wizard locally](https://docs.getdbt.com/docs/dbt-ai/wizard-quickstart.md).

Use dbt Wizard CLI to:

* Investigate lineage, model health, and downstream impact
* Debug failed runs and propose fixes
* Build or refactor models from plain-language prompts
* Update SQL, YAML, refs, tests, and docs together
* Validate changes before review
* Run non-interactively in CI with `exec` and `review`

For more examples, visit [Use cases and examples](https://docs.getdbt.com/docs/dbt-ai/wizard-use-cases.md).

Best practices for using dbt Wizard

For recommended workflows on real project tasks, refer to [How to use dbt Wizard in your dbt project](https://docs.getdbt.com/best-practices/how-to-use-wizard/wizard-1-intro.md).

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

* [Configure dbt platform](https://docs.getdbt.com/docs/platform/wizard-byok-platform.md) integrations in account settings.
* [Configure BYOK for the CLI](https://docs.getdbt.com/docs/dbt-ai/wizard-byok.md) by running `wizard providers configure PROVIDER_NAME` and follow the prompts.

Looking for the in-platform experience? Visit [About dbt Wizard in the dbt platform](https://docs.getdbt.com/docs/platform/wizard-platform.md).

## Use dbt Wizard locally[​](#use-dbt-wizard-locally "Direct link to Use dbt Wizard locally")

[![](/img/icons/wizard.svg)](https://docs.getdbt.com/docs/dbt-ai/wizard-quickstart.md)

#### [Use dbt Wizard locally](https://docs.getdbt.com/docs/dbt-ai/wizard-quickstart.md)

[Install dbt Wizard locally, complete first-run onboarding, and send your first prompt.](https://docs.getdbt.com/docs/dbt-ai/wizard-quickstart.md)

[![](/img/icons/wizard.svg)](https://docs.getdbt.com/docs/dbt-ai/wizard-cli.md)

#### [Install and update](https://docs.getdbt.com/docs/dbt-ai/wizard-cli.md)

[Install, verify, update, and uninstall the dbt Wizard CLI on your machine.](https://docs.getdbt.com/docs/dbt-ai/wizard-cli.md)

[![](/img/icons/wizard.svg)](https://docs.getdbt.com/docs/dbt-ai/wizard-byok.md)

#### [BYOK configuration](https://docs.getdbt.com/docs/dbt-ai/wizard-byok.md)

[Bring your own API key and choose a supported AI provider.](https://docs.getdbt.com/docs/dbt-ai/wizard-byok.md)

[![](/img/icons/wizard.svg)](https://docs.getdbt.com/docs/dbt-ai/wizard-skills.md)

#### [Use skills](https://docs.getdbt.com/docs/dbt-ai/wizard-skills.md)

[Give dbt Wizard reusable instructions for your project.](https://docs.getdbt.com/docs/dbt-ai/wizard-skills.md)

[![](/img/icons/wizard.svg)](https://docs.getdbt.com/docs/dbt-ai/wizard-subagents.md)

#### [Use subagents](https://docs.getdbt.com/docs/dbt-ai/wizard-subagents.md)

[Organize long-running dbt Wizard work into focused agent threads.](https://docs.getdbt.com/docs/dbt-ai/wizard-subagents.md)

[![](/img/icons/wizard.svg)](https://docs.getdbt.com/docs/dbt-ai/wizard-mcp.md)

#### [Use MCP servers](https://docs.getdbt.com/docs/dbt-ai/wizard-mcp.md)

[Connect the dbt Wizard CLI to MCP servers for more tools and context.](https://docs.getdbt.com/docs/dbt-ai/wizard-mcp.md)

[![](/img/icons/wizard.svg)](https://docs.getdbt.com/docs/dbt-ai/wizard-headless.md)

#### [Headless mode](https://docs.getdbt.com/docs/dbt-ai/wizard-headless.md)

[Run dbt Wizard in scripts and CI with exec, review, and other headless workflows.](https://docs.getdbt.com/docs/dbt-ai/wizard-headless.md)

[![](/img/icons/wizard.svg)](https://docs.getdbt.com/docs/dbt-ai/wizard-migrate.md)

#### [Migrate to dbt Wizard](https://docs.getdbt.com/docs/dbt-ai/wizard-migrate.md)

[Move Claude Code project context, skills, and settings into dbt Wizard.](https://docs.getdbt.com/docs/dbt-ai/wizard-migrate.md)

[![](/img/icons/wizard.svg)](https://docs.getdbt.com/docs/dbt-ai/wizard-cli-reference.md)

#### [Command reference](https://docs.getdbt.com/docs/dbt-ai/wizard-cli-reference.md)

[Curated reference for common wizard subcommands and global flags.](https://docs.getdbt.com/docs/dbt-ai/wizard-cli-reference.md)

[![](/img/icons/wizard.svg)](https://docs.getdbt.com/docs/dbt-ai/wizard-slash-commands.md)

#### [Slash command reference](https://docs.getdbt.com/docs/dbt-ai/wizard-slash-commands.md)

[Full reference for interactive TUI slash commands.](https://docs.getdbt.com/docs/dbt-ai/wizard-slash-commands.md)

[![](/img/icons/wizard.svg)](https://docs.getdbt.com/docs/dbt-ai/wizard-config.md)

#### [Config reference](https://docs.getdbt.com/docs/dbt-ai/wizard-config.md)

[Configure agent runtime defaults and per-project dbt Wizard settings.](https://docs.getdbt.com/docs/dbt-ai/wizard-config.md)

[![](/img/icons/wizard.svg)](https://docs.getdbt.com/docs/dbt-ai/wizard-telemetry.md)

#### [Data & privacy](https://docs.getdbt.com/docs/dbt-ai/wizard-telemetry.md)

[Understand what dbt Wizard CLI collects and how to opt out.](https://docs.getdbt.com/docs/dbt-ai/wizard-telemetry.md)

See it in action and share your feedback

Want to see dbt Wizard in action? Check out the [demo video](https://www.youtube.com/watch?v=-lIzh1xQWMA).

We'd love to hear how dbt Wizard is working for you. Share your feedback by either running the `/feedback` slash command in your interactive terminal session or by going to the [#dbt-wizard](https://getdbt.slack.com/archives/C0B6KLW6T26) channel in the [dbt Community Slack](https://docs.getdbt.com/community/join?version=2.0).

Thanks so much for your help in improving dbt Wizard and dbt data development!
