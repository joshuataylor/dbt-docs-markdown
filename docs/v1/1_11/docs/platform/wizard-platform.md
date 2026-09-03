# About dbt Wizard in the dbt platform [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

dbt platform | Usage-based

dbt Wizard is dbt's AI agent in the dbt platform, helping teams investigate, change, validate, and ship trusted dbt work with warehouse-aware grounding.

dbt Wizard is more than a general coding agent with access to dbt. Built for governed data development in dbt, it understands lineage, documentation, tests, and semantic definitions, and accounts for dev builds, compute, run time, and post-build inspection. Its suggestions are grounded in your project's actual data *and* context.

AI features are being enabled by default. They're already on for new accounts and are rolling out soon to existing accounts. If your organization opted out, they'll remain off. Admins can [turn AI off or back on and configure providers](./manage-dbt-ai.md) anytime.

Wizard usage and billing

From September 1st, 2026, dbt Wizard usage is metered per token against your account's usage credits. All credit amounts are per account, not per user. Enterprise and Enterprise+ accounts get monthly credits. Developer and Starter plans start with a 30-day trial and $100 in credits, as do CLI users via a free dbt account.

Refer to [Trial and billing](../dbt-ai/pricing-billing/trial-and-billing.md) for what each plan gets, spend limits, and paid access.

## Where you can use dbt Wizard

* **[dbt Wizard home tab](./wizard-home.md):** Ask questions about your project, generate changes, review the diff, and run validations — all in one place.
* **[Studio IDE](../dbt-ai/wizard-ide.md):** Work with dbt Wizard alongside the code editor, console, and file explorer.
* **[Terminal (CLI)](../dbt-ai/wizard-cli.md):** Use the same agent from your terminal, with or without a dbt platform account.

You can teach dbt Wizard your team's conventions with [skills](../dbt-ai/wizard-platform-skills.md), hand off bigger jobs to [subagents](../dbt-ai/wizard-platform-subagents.md), or connect your project to other AI tools with the [dbt platform MCP server](../dbt-ai/wizard-platform-mcp.md).

## What you can do

* Ask project-aware questions and get answers grounded in your project's context
* Generate documentation, semantic models, tests, and metrics
* Build or refactor models from plain-language prompts
* Review file changes as diffs before you save them
* Run end-to-end tasks in [agent mode](../dbt-ai/wizard-ide.md#agent-modes), either approving each file change or letting dbt Wizard edit automatically
* Ask questions of production data in [Explore mode](../dbt-ai/wizard-ide.md#agent-modes), including [read-only users](./wizard-read-only-users.md) with no development access
* Follow along in the wayfinder bar, which shows your current project and branch and guides you through Git tasks like committing files or creating a branch
* Get consistent output from [dbt Agent Skills](https://github.com/dbt-labs/dbt-agent-skills), which encode dbt best practices out of the box

For more examples, visit [Use cases and examples](../dbt-ai/wizard-use-cases.md).

Best practices for using dbt Wizard

Most of the workflows in [How to use dbt Wizard in your dbt project](../../best-practices/how-to-use-wizard/wizard-1-intro.md) apply here too — the prompts work the same in Studio IDE and the dbt Wizard home tab.

## Supported AI providers

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

## Open dbt Wizard

1. Sign in to dbt platform.

2. Select the dbt Wizard icon in the navigation menu, or open it in the Studio IDE.

3. Enter a prompt. Try one of these to get a feel for it:

   * `summarize what this project does`
   * `which models in this project have no tests?`
   * `add not_null and unique tests to the primary key of stg_customers`

4. ..and that's it 🎉!

![dbt Wizard in the Studio IDE refactoring a model and displaying the lineage inside the chat interface.](/img/docs/dbt-platform/wizard-ide-refactor-lineage.png?v=2 "dbt Wizard in the Studio IDE refactoring a model and displaying the lineage inside the chat interface.")dbt Wizard in the Studio IDE refactoring a model and displaying the lineage inside the chat interface.

![Wizard's final refactor result displayed as a diff in the Studio IDE](/img/docs/dbt-platform/wizard-ide-refactor-diff.png?v=2 "Wizard's final refactor result displayed as a diff in the Studio IDE")Wizard's final refactor result displayed as a diff in the Studio IDE

For more prompt ideas, refer to the [prompt cookbook](../../guides/prompt-cookbook.md).

If dbt Wizard isn't available, confirm that your plan is eligible and that an admin has [enabled account access to AI features](./manage-dbt-ai.md#manage-ai-features).

## Trial and usage credits

Every prompt you send to dbt Wizard uses AI model tokens. What you pay for those tokens depends on your plan and whether you use a dbt Labs-managed model or your own key. If you [bring your own key](./wizard-byok-platform.md), your AI provider bills you directly and your usage doesn't draw from your consumption pool.

[Legacy team plans](./billing/plans-and-billing.md#legacy-plans) don't have access to dbt Wizard. You can move to a [Starter, Enterprise, or Enterprise+ plan](https://www.getdbt.com/pricing) to use it.

For steps on how to start your trial and manage your spend, refer to [Start your trial](../dbt-ai/pricing-billing/trial-and-billing.md#start-your-trial) and [Manage your spend limit](../dbt-ai/pricing-billing/trial-and-billing.md#manage-your-spend-limit).

To choose a provider or bring your own key, refer to [Manage AI features](./manage-dbt-ai.md#configure-ai-provider).

## Other AI features in dbt platform

dbt platform also has AI features that work separately from dbt Wizard:

* [dbt Copilot](../dbt-ai/copilot-overview.md) generates SQL, documentation, tests, and semantic models in one click in Studio IDE, Canvas, and Insights.
* The [dbt Support Assistant](../dbt-support.md?version=2#ask-dbt-support-assistant) answers product questions and helps you open a ticket. Contact the [dbt Support team](mailto:support@getdbt.com) for more info.

 What's the difference between dbt Wizard and dbt Copilot?

dbt Wizard is the recommended agent for dbt work

dbt Wizard is the recommended AI agent for governed data development in dbt. It handles the full development lifecycle — investigation, building, validation, and shipping — grounded in your dbt project's lineage, tests, contracts, and metric definitions.

Refer to [dbt AI FAQs](../dbt-ai/dbt-ai-faqs.md#is-dbt-wizard-the-same-as-dbt-copilot), [Billing](../dbt-ai/wizard-billing-faqs.md), and [dbt's Terms of Use](https://www.getdbt.com/terms-of-use) for more information.

## Related docs

* [dbt Wizard home tab](./wizard-home.md)
* [dbt Wizard in Studio IDE](../dbt-ai/wizard-ide.md)
* [Manage AI features in dbt platform](./manage-dbt-ai.md) — admin setup for AI access and providers
* [Configure BYOK in dbt platform](./wizard-byok-platform.md)
* [Models and pricing](../dbt-ai/pricing-billing/overview.md)
* [Data & Privacy in dbt platform](../dbt-ai/dbt-ai-faqs.md#privacy-and-data)
