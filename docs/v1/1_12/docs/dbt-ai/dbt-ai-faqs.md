# dbt AI FAQs

Answers to common questions about dbt AI features, including dbt Wizard and dbt Copilot.

dbt Wizard is an AI-powered assistant fully integrated into your dbt experience that handles the tedious tasks, speeds up workflows, and ensures consistency, helping you deliver exceptional data products faster.

dbt Labs is committed to protecting your privacy and data. This page provides information about how dbt Wizard handles your data. For more information, check out the [dbt Labs AI development principles](https://www.getdbt.com/legal/ai-principles) page.

## Overview

 What is dbt Wizard?

dbt Wizard is the latest and recommended agentic experience for governed data development in dbt, available in both the dbt platform and the terminal CLI. It helps teams ship trusted dbt changes faster and with less risk by understanding project context, routing to the right dbt tools, validating changes, and supporting review before changes are persisted.

Use dbt Wizard to investigate lineage and downstream impact, generate or refactor SQL from natural-language prompts, create [documentation](../build/documentation.md), [data tests](../build/data-tests.md), [metrics](../build/metrics-overview.md), and [semantic models](../build/semantic-models.md), and validate changes with warehouse awareness.

In the dbt platform, dbt Wizard is available in the [Studio IDE](../platform/studio-ide/develop-studio-ai.md) and the [dbt Wizard home tab](../platform/wizard-home.md). dbt Copilot is available in [Canvas](../platform/use-canvas.md) and [Insights](../explore/dbt-insights.md).

In the CLI, you can use dbt Wizard from your terminal for local development workflows.

 Where can I find dbt Wizard?

dbt Wizard is available in the dbt platform and as a terminal CLI.

* In the platform, you can use dbt Wizard in the [Studio IDE](./wizard-ide.md) for governed data development in dbt.
* In the CLI, use the [dbt Wizard CLI](./wizard-cli.md) for local development and automation.

To use dbt Wizard in the platform, you need any dbt [platform account](https://www.getdbt.com/contact).

All dbt platform plans have access to dbt Wizard in Studio IDE and the [home tab](../platform/wizard-home.md).

AI features are enabled by default. Admins can [turn them off or back on anytime](../platform/manage-dbt-ai.md).

 What are the benefits of using dbt Wizard?

dbt Wizard helps teams ship trusted dbt changes faster and with less risk. Use it to:

* Ask project-aware questions and investigate lineage, dependencies, and downstream impact.
* Generate or refactor SQL from natural-language prompts.
* Generate documentation, tests, metrics, and semantic models.
* Validate changes with warehouse awareness before review.
* Review proposed file changes as diffs before they are persisted.

dbt Wizard is built into dbt experiences with dbt governance, privacy, and security controls.

 Is dbt Wizard the same as dbt Copilot?

No, dbt Wizard and dbt Copilot are separate products.

dbt Wizard is the latest and next generation of agentic product available in the dbt platform and as a CLI. It uses your project context to help you develop governed dbt changes faster. Think of it like a smart AI agent that has a map of your project. Instead of having to read through each file and understand the context, it can answer questions and help you develop *and* validate your changes faster.

dbt Copilot features for those who have access to them, include quick-action buttons in Studio IDE, the Copilot pane in Insights and Canvas.

 Can I use my existing dbt Copilot action allotment with dbt Wizard?

No, dbt Copilot actions apply only to dbt Copilot usage. Refer to [dbt Wizard billing and AI access FAQs](./wizard-billing-faqs.md).

## Availability

 Who has access to dbt Wizard?

**In the dbt platform**:

When enabled by an admin, dbt Wizard is available to users with a dbt [developer license](../platform/manage-access/seats-and-users.md) on any [dbt platform account](https://www.getdbt.com/contact).

**In the CLI**:

dbt Wizard CLI uses dbt Labs-managed models (OpenAI, Anthropic, or open weight models), billed through dbt's spend-limit billing. You can also bring your own API key or credentials for a supported provider using [BYOK](./wizard-byok.md): OpenAI, Anthropic, Azure AI Foundry, AWS Bedrock, Google Gemini, or Snowflake Cortex (preview). BYOK is a good backup option if you want to manage AI costs directly — token costs are billed directly by whichever provider you choose. Install and configure the CLI on your local machine.

Refer to [Use dbt Wizard locally](./wizard-quickstart.md) for more information.

 Is dbt Wizard available for all deployment types?

Yes, dbt Wizard is deployed everywhere, including [multi-tenant and single-tenant deployments](../platform/about-platform/access-regions-ip-addresses.md).

## How it works

 What data/code is used to train the AI model supporting dbt Wizard?

dbt Wizard is supported by dbt Labs-managed models (OpenAI, Anthropic, or open weight models), or by several third-party pre-trained AI models at your discretion (BYOK OpenAI, BYOK Anthropic, BYOK Azure AI Foundry, and so on). When using managed OpenAI, our agreement with OpenAI prohibits OpenAI from retaining your data persistently. Refer to our [dbt Labs AI principles page](https://www.getdbt.com/legal/ai-principles) for more information.

 Which AI model providers does dbt Wizard use?

In the dbt platform, dbt Wizard uses a managed OpenAI model by default. dbt Labs also offers managed open weight models. On any plan, you can also [bring your own provider keys](../platform/wizard-byok-platform.md) for OpenAI, Anthropic, or Azure AI Foundry.

The [dbt Wizard CLI](./wizard-cli.md) supports the same dbt Labs-managed models, or OpenAI, Anthropic, Azure AI Foundry, AWS Bedrock, Google Gemini, and Snowflake Cortex (preview) in bring-your-own-key mode. Refer to [Configure BYOK](./wizard-byok.md) and [Supported AI providers](./pricing-billing/overview.md#supported-ai-providers) for more information.

For how model choice affects cost, which models draw from your consumption pool, and how BYOK billing works, refer to [dbt Wizard billing and access FAQs](./wizard-billing-faqs.md).

Refer to the [Model Provider Rate table](https://www.getdbt.com/legal/dbt-wizard-token-costs-by-model) and [Service Consumption Table](https://www.getdbt.com/legal/service-consumption-table) for more information.

 Do we support BYOK (bring your own key) at the project level?

In dbt platform, the dbt Wizard BYOK option is currently an account-only configuration. However, there may be a future where we make this configurable on a project-level. BYOK is a good option if you want to manage AI costs directly, rather than using a dbt Labs-managed model.

dbt Wizard CLI supports BYOK locally for OpenAI, Anthropic, Azure AI Foundry, AWS Bedrock, Google Gemini, and Snowflake Cortex (preview).

## Privacy and data

This section covers dbt Wizard in the dbt platform. For what the CLI collects and how to opt out, refer to [dbt Wizard CLI data use and telemetry](./wizard-telemetry.md).

 Does dbt Wizard access my warehouse data?

dbt Wizard can run dbt commands and queries on your behalf, and every query needs your explicit permission first. When a query runs, dbt Wizard sends those results — which may include row-level data — to the AI provider so it can respond in your session.

For dbt-managed AI providers, we have zero data retention (ZDR) agreements in place that prevents the provider from retaining or using this data for training. If you bring your own AI provider (BYOK), that provider's terms will govern retention and training. Always review AI output for accuracy.

Refer to [Model Provider Rate table](https://www.getdbt.com/legal/dbt-wizard-token-costs-by-model) and [Service Consumption Table](https://www.getdbt.com/legal/service-consumption-table) for more information.

 Does dbt Wizard store or use personal data?

dbt Wizard stores your conversation history — including your prompts, responses, and any query results returned during your session — so you can revisit past chats. Conversation history is retained for 90 days; feedback you submit on a dbt Wizard conversation is retained for 400 days. You can delete your conversation history or feedback at any time in the product.

You control the information you submit to dbt Wizard. dbt Labs does not use your prompts, conversation history, command results, or feedback to train AI models.

 Is my data used by dbt Labs to train AI models?

No. dbt Labs does not use customer content processed by dbt Wizard — including warehouse query results, prompts, or conversation history — for AI model training. A zero data retention (ZDR) policy is also in place with AI providers, which prevents training on the provider side as well.

 Does dbt Labs share my personal data with third parties?

dbt Labs only shares client personal information as needed to perform the services, under client instructions, or for legal, tax, or compliance reasons.

 Can dbt Wizard data be deleted upon client written request?

Yes. dbt Wizard conversation history is retained for 90 days by default, feedback you submit on a dbt Wizard conversation is retained for 400 days, and you can delete this information in the product at any time. To the extent a client identifies personal or sensitive information uploaded to dbt Labs systems, that data can be deleted within 30 days of written request.

 Does dbt Labs own the output generated by dbt Wizard?

dbt Labs will not dispute your ownership of any output (e.g., code or artifacts) that are unique to your company generated when you use dbt Wizard. Your code will not be used to train AI models for the benefit of dbt Labs or other third parties, including other dbt Labs customers.

 Does dbt Labs have terms in place?

dbt Wizard is governed by our [Terms of Use](https://www.getdbt.com/terms-of-use). In the event clients prefer additional terms, clients may enter into the presigned AI & Beta Addendum (the dbt Labs signature will be dated as of the date the client signs). Contact your account manager for more information.

Clients who signed with terms after January 2024 don't need additional terms prior to enabling dbt Wizard. Longer term clients have also protected their data through confidentiality and data deletion obligations. In the event clients prefer additional terms, clients may enter into the presigned AI & Beta Addendum (the dbt Labs signature will be dated as of the date the client signs).

## Considerations

 What are the considerations for using dbt Wizard?

* dbt Wizard is not available in the dbt API.

Future releases are planned that may bring dbt Wizard to even more parts of the dbt application.

## dbt Wizard allowlisting URLs

 Allowlisting URLs

dbt Wizard doesn't specifically block AI-related URLs. However, if your organization use endpoint protection platforms, firewalls, or network proxies (such as Zscaler), you may encounter the following issues with dbt Wizard:

* Block unknown or AI-related domains.
* Break TLS/SSL traffic to inspect it.
* Disallow specific ports or services.

We recommend the following URLs to be allowlisted:

**For dbt Wizard in the IDE**:

* `/api/ide/accounts/${accountId}/develop/${developId}/ai/generate_generic_tests/...`
* `/api/ide/accounts/${accountId}/develop/${developId}/ai/generate_documentation/...`
* `/api/ide/accounts/${accountId}/develop/${developId}/ai/generate_semantic_model/...`
* `/api/ide/accounts/${accountId}/develop/${developId}/ai/generate_inline`
* `/api/ide/accounts/${accountId}/develop/${developId}/ai/generate_metrics/...`
* `/api/ide/accounts/${accountId}/develop/${developId}/ai/track_response`

**For dbt Copilot in Canvas**:

* `/api/private/visual-editor/v1/ai/llm-generate`
* `/api/private/visual-editor/v1/ai/track-response`
* `/api/private/visual-editor/v1/files/${fileId}/llm-generate-dag-through-chat`
