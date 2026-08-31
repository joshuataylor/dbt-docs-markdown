# dbt Copilot

dbt platform | Starter, Enterprise, Enterprise+

dbt Copilot helps you generate SQL, documentation, tests, and semantic models in the dbt platform.

info

dbt Wizard is the new and recommended AI agent for governed data development in dbt. It handles the full development lifecycle — investigation, building, validation, and shipping — grounded in your dbt project's lineage, tests, contracts, and metric definitions.

dbt Copilot is separate from dbt Wizard and is dbt's inline AI assistance experience, providing single-click generation of SQL, documentation, tests, and semantic models in Studio IDE, Canvas, and Insights.

Refer to [dbt AI FAQs](./dbt-ai-faqs.md#is-dbt-wizard-the-same-as-dbt-copilot), [Billing](../platform/billing.md), and [dbt's Terms of Use](https://www.getdbt.com/terms-of-use) for more information.

*The earlier version of dbt Copilot in the Studio IDE is available only to a limited set of accounts. dbt Wizard is available to all accounts and is the recommended way to develop with AI in the Studio IDE — it covers everything dbt Copilot's quick actions did, plus multi-step changes with built-in validation.*

## Where to access dbt Copilot

dbt Copilot is available across the following experiences in the dbt platform. Refer to the links for more info on how to use each experience.

* [dbt Copilot in Canvas](../platform/build-canvas-copilot.md): Build visual models using natural language prompts in Canvas
* [dbt Copilot in Insights](./analyst-agent.md): Chat with your data and get answers powered by the dbt Semantic Layer in Insights

## Action limits by plan

dbt Copilot usage is metered in actions — one completed AI request counts as one action. Each plan includes a monthly action allotment per license:

| Plan        | Actions per month |
| ----------- | ----------------- |
| Developer   | ❌                |
| Starter     | 100               |
| Enterprise  | 5,000             |
| Enterprise+ | 10,000            |

Enterprise and Enterprise+ limits don't apply if you [bring your own key (BYOK)](../platform/wizard-byok-platform.md), since your AI provider bills that usage directly. Legacy Enterprise-tier plans enrolled before May 1, 2025 have a 1,000 action limit.

Refer to [dbt AI usage](../platform/billing/dbt-ai-usage.md) for what counts as an action, what happens when you hit the limit, and how to check your usage.

## Considerations

* dbt Copilot is a separate experience from dbt Wizard. For agentic, full-lifecycle AI development, use [dbt Wizard](../platform/wizard-overview.md).
* Certain features are only available on Enterprise and Enterprise+ plans. Refer to [Billing](../platform/billing.md) for details.
* dbt Copilot doesn't yet support generating semantic models with the latest YAML spec.
* dbt Copilot requires AI features to be [enabled](../platform/enable-dbt-ai.md) for your account.
