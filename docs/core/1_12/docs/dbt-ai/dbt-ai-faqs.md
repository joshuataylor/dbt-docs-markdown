# dbt AI FAQs

Answers to common questions about dbt AI features, including dbt Wizard and dbt Copilot.

*dbt Wizard is available in Studio IDE as a public preview feature, and as a standalone beta feature across the managed dbt platform. However, certain customers may have disabled experimental features, in which case, they can use Wizard CLI via terminal-access and will continue to have access to dbt Copilot until dbt Wizard is released as generally available to all customers. [Contact dbt Support](mailto:support@getdbt.com) with any questions.*

Please contact dbt Support with any questions

dbt Wizard is an AI-powered assistant fully integrated into your dbt experience that handles the tedious tasks, speeds up workflows, and ensures consistency, helping you deliver exceptional data products faster.

dbt Labs is committed to protecting your privacy and data. This page provides information about how dbt Wizard handles your data. For more information, check out the [dbt Labs AI development principles](https://www.getdbt.com/legal/ai-principles) page.

## Overview[​](#overview "Direct link to Overview")

 What is dbt Wizard?

dbt Wizard is the latest and recommended agentic experience for governed data development in dbt, available in both the dbt platform and the terminal CLI. It helps teams ship trusted dbt changes faster and with less risk by understanding project context, routing to the right dbt tools, validating changes, and supporting review before changes are persisted.

Use dbt Wizard to investigate lineage and downstream impact, generate or refactor SQL from natural-language prompts, create [documentation](https://docs.getdbt.com/docs/build/documentation.md), [data tests](https://docs.getdbt.com/docs/build/data-tests.md), [metrics](https://docs.getdbt.com/docs/build/metrics-overview.md), and [semantic models](https://docs.getdbt.com/docs/build/semantic-models.md), and validate changes with warehouse awareness.

In the dbt platform, dbt Wizard is available in experiences such as the [Studio IDE](https://docs.getdbt.com/docs/platform/studio-ide/develop-studio-ai.md), [Canvas](https://docs.getdbt.com/docs/platform/use-canvas.md), and [Insights](https://docs.getdbt.com/docs/explore/dbt-insights.md).

In the CLI, you can use dbt Wizard from your terminal for local development workflows.

 Where can I find dbt Wizard?

dbt Wizard is available in the dbt platform and as a terminal CLI.

* In the platform, you can use dbt Wizard in the [Studio IDE](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md) for governed data development in dbt.
* In the CLI, use the [dbt Wizard CLI](https://docs.getdbt.com/docs/dbt-ai/about-dbt-wizard-cli.md) for local development and automation.

To use dbt Wizard in the platform, you need a dbt [Starter, Enterprise, or Enterprise+ account](https://www.getdbt.com/contact), and an admin must [enable dbt Wizard](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md) for your account.

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

dbt Wizard is the new and recommended agentic product available in the dbt platform and as a CLI. It uses your project context to help you develop governed dbt changes faster. Think of it like a smart AI agent that has a map of your project. Instead of having to read through each file and understand the context, it can answer questions and help you develop *and* validate your changes faster.

dbt Copilot features include quick-action buttons in Studio IDE, the Copilot pane in Insights and Canvas.

 Can I use my existing dbt Copilot action allotment with dbt Wizard?

Yes, as a temporary compatibility bridge through July 1. After July 1, this bridge ends.

## Availability[​](#availability "Direct link to Availability")

 Who has access to dbt Wizard?

**In the dbt platform**:

When enabled by an admin, dbt Wizard is available to users with a dbt [developer license](https://docs.getdbt.com/docs/platform/manage-access/seats-and-users.md) on [Starter, Enterprise, and Enterprise+ accounts](https://www.getdbt.com/contact).

**In the CLI**:

For dbt Wizard CLI, bring your own API key or credentials for a supported provider using [BYOK](https://docs.getdbt.com/docs/dbt-ai/wizard-byok.md): OpenAI, Anthropic, Azure AI Foundry, AWS Bedrock, Google Gemini, or Snowflake Cortex (preview). Install and configure the CLI on your local machine. BYOK means any token costs will be billed directly by whichever provider you choose.

Refer to [Quickstart](https://docs.getdbt.com/docs/dbt-ai/wizard-quickstart.md) for more information.

 Is dbt Wizard available for all deployment types?

Yes, dbt Wizard is powered by ai-codegen-api, which is deployed everywhere including [multi-tenant and single-tenant deployments](https://docs.getdbt.com/docs/platform/about-platform/access-regions-ip-addresses.md).

## How it works[​](#how-it-works "Direct link to How it works")

 What data/code is used to train the AI model supporting dbt Wizard?

dbt Wizard is supported by several third-party pre-trained AI models at your discretion. (managed OpenAI, BYOK OpenAI, BYOK Anthropic, or BYOK Azure AI Foundry). When using managed OpenAI, our agreement with OpenAI prohibits OpenAI from retaining your data persistently. Refer to our [dbt Labs AI principles page](https://www.getdbt.com/legal/ai-principles) for more information.

 Which AI model providers does dbt Wizard use?

In the dbt platform, dbt Wizard supports managed OpenAI, BYOK OpenAI, BYOK Anthropic, and BYOK Azure AI Foundry. By default, accounts use managed OpenAI. Enterprise-tier accounts can [bring their own provider keys](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md#configure-ai-provider).

The [dbt Wizard CLI](https://docs.getdbt.com/docs/dbt-ai/wizard-cli.md) supports OpenAI, Anthropic, Azure AI Foundry, AWS Bedrock, Google Gemini, and Snowflake Cortex (preview) in bring-your-own-key mode. Refer to [Configure BYOK](https://docs.getdbt.com/docs/dbt-ai/wizard-byok.md) and [Supported AI providers](https://docs.getdbt.com/docs/dbt-ai/wizard-byok.md#supported-ai-providers) for more information.

 Do we support BYOK (bring your own key) at the project level?

In dbt platform, the dbt Wizard BYOK option is currently an account-only configuration. However, there may be a future where we make this configurable on a project-level.

dbt Wizard CLI supports BYOK locally for OpenAI, Anthropic, Azure AI Foundry, AWS Bedrock, Google Gemini, and Snowflake Cortex (preview).

## Privacy and data[​](#privacy-and-data "Direct link to Privacy and data")

 Does dbt Wizard store or use personal data?

The user clicks the dbt Wizard button. Aside from authentication, it works without personal data, but the user controls what is input into dbt Wizard.

 Can dbt Wizard data be deleted upon client written request?

To the extent client identifies personal or sensitive information uploaded by or on behalf of client to dbt Labs systems by the user in error, such data can be deleted within 30 days of written request.

 Does dbt Labs own the output generated by dbt Wizard?

dbt Labs will not dispute your ownership of any output (e.g., code or artifacts) that are unique to your company generated when you use dbt Wizard. Your code will not be used to train AI models for the benefit of dbt Labs or other third parties, including other dbt Labs customers.

 Does dbt Labs have terms in place?

dbt Wizard is governed by our [Terms of Use](https://www.getdbt.com/terms-of-use). In the event clients prefer additional terms, clients may enter into the presigned AI & Beta Addendum (the dbt Labs signature will be dated as of the date the client signs). Contact your account manager for more information.

Clients who signed with terms after January 2024 don't need additional terms prior to enabling dbt Wizard. Longer term clients have also protected their data through confidentiality and data deletion obligations. In the event clients prefer additional terms, clients may enter into the presigned AI & Beta Addendum (the dbt Labs signature will be dated as of the date the client signs).

## Considerations[​](#considerations "Direct link to Considerations")

 What are the considerations for using dbt Wizard?

* dbt Wizard is not available in the dbt API.

Future releases are planned that may bring dbt Wizard to even more parts of the dbt application.

## dbt Wizard allowlisting URLs[​](#dbt-wizard-allowlisting-urls "Direct link to dbt Wizard allowlisting URLs")

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

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
