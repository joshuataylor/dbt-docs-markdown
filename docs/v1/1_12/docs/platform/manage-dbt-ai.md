# Manage AI features in dbt platform

dbt platform

Manage AI features in dbt platform by turning AI features on or off, choosing which AI provider to use, and controlling spend.

The details on this page are generally for admin tasks. If you're looking for what dbt Wizard does and where you can use it, start with [About dbt Wizard in dbt platform](./wizard-platform.md).

Wizard usage and billing

From September 1st, 2026, dbt Wizard usage is metered per token against your account's usage credits. All credit amounts are per account, not per user. Enterprise and Enterprise+ accounts get monthly credits. Developer and Starter plans start with a 30-day trial and $100 in credits, as do CLI users via a free dbt account.

Refer to [Trial and billing](../dbt-ai/pricing-billing/trial-and-billing.md) for what each plan gets, spend limits, and paid access.

## Prerequisites

* A [dbt platform account](https://www.getdbt.com/pricing) on Developer, Starter, Enterprise, or Enterprise+ plans

  * [Legacy team plans](./billing/plans-and-billing.md#legacy-plans) don't have access to dbt Wizard and we recommend moving to a [Starter, Enterprise, or Enterprise+ plan](https://www.getdbt.com/pricing) to use it and other features.
  * Certain features like [natural prompts in Canvas](./build-canvas-copilot.md) are only available on Enterprise and Enterprise+ plans.

* dbt admin permissions to change account settings and configure providers.

* A development environment on a supported [release track](../dbt-versions/dbt-release-tracks.md) to receive ongoing updates.

## Manage account access to AI features

Eligible accounts, including new ones, have AI features on by default, so most users can open dbt Wizard without an admin changing anything first. Admins with governance or compliance requirements can turn AI off or back on anytime by following these steps:

1. Navigate to **Account settings** in the navigation menu.
2. Under **Settings**, confirm the account you want to change.
3. Click **Edit** in the top right corner.
4. Turn **Enable account access to AI features** off to disable access or on to enable it.
5. Click **Save**.

Turning AI features off applies to the whole account, including dbt Wizard and dbt Copilot. The [dbt Support Assistant](../dbt-support.md?version=2#ask-dbt-support-assistant) is managed separately by dbt Labs (contact the [dbt Support team](mailto:support@dbtlabs.com) for more info).

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

dbt Copilot supports a smaller set of providers. Refer to [dbt Copilot](../dbt-ai/copilot-overview.md#configure-ai-provider-for-dbt-copilot) for its providers and setup steps.

## Configure AI provider

A dbt platform account on any plan can use a dbt managed provider to get started right away or configure a custom AI provider by BYOK. If you use BYOK, you will incur API calls and associated charges from that provider.

The following instructions explain how to configure a dbt Labs managed or BYOK AI provider for dbt Wizard. dbt managed AI providers are administered by dbt Labs and don’t require a user-provided API key. The AI providers shown in **Account settings** may change as dbt Labs adds managed models. For the current list of models and providers, refer to the [Model Provider Rate table](https://www.getdbt.com/legal/dbt-wizard-token-costs-by-model).

**To configure a provider:**

1. Click your account name and select **Account settings** in the side menu.
2. Under **Settings**, click **AI features**.
3. Under **AI providers**, click **Edit** to configure the AI integration.
4. For each provider, select your **Key management** option from the dropdown, then follow these steps for your provider below.

### Anthropic

**Managed by dbt Labs**

Use this option as a managed alternative to the default OpenAI model.

1. Unless not selected, select **dbt Labs** from the list to use dbt Labs' managed Anthropic key.
2. Click **Save**.

**Managed by you (BYOK)**

1. Select **Anthropic** from the options.
2. Enter your Anthropic API key.
3. Click **Save**.

Embedding limitations

When using an Anthropic API key, dbt continues to use the dbt Labs-managed OpenAI key for embeddings in `text_to_sql` MCP tools, since Anthropic doesn't natively provide embeddings.

### OpenAI

**Managed by dbt Labs**

This is the default option and use it if you want to use dbt managed AI provider keys. Refer to [Models and pricing](../dbt-ai/pricing-billing/overview.md?version=2.0) for more information.

1. Unless not selected, select the toggle for **dbt Labs** to use a dbt Labs' managed key.
2. Click **Save**.

![Example of the dbt Labs integration page](/img/docs/dbt-platform/account-integration-dbtlabs.png?v=2 "Example of the dbt Labs integration page")Example of the dbt Labs integration page

**Managed by you (BYOK)**

1. Select **OpenAI** from the options.
2. Enter your OpenAI API key.
3. Click **Save**.

![Example of the OpenAI integration page](/img/docs/dbt-platform/account-integration-openai.png?v=2 "Example of the OpenAI integration page")Example of the OpenAI integration page

Data residency limitation

OpenAI projects with [data residency controls](https://platform.openai.com/docs/guides/your-data#data-residency-controls) enabled and configured for the United States (project region set to US) don't currently support BYOK. These projects can only use the API key in the dbt platform configuration. Specifying custom endpoints required for data residency isn’t yet supported, and we’re evaluating a solution for this.

To use BYOK, ensure your OpenAI project doesn’t have data residency controls enabled. Projects without project region settings will use the standard OpenAI endpoint (`https://api.openai.com`) and support BYOK.

* For BYOK, enable the latest text generation models as well as the `text-embedding-3-small` model.
* Ensure your project doesn't have data residency controls enabled. Projects without project region settings use the standard OpenAI endpoint (`https://api.openai.com`) and support BYOK.

### Azure AI Foundry

**Managed by you only (BYOK)**

To learn about deploying models on Azure, refer to [Deploy models on Azure OpenAI](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/deploy-models-openai) and [Azure AI Foundry](https://learn.microsoft.com/en-us/azure/ai-foundry/).

Configure credentials for your Azure AI Foundry deployment in dbt as follows:

1. Locate your deployment details in the Azure portal, either in Azure AI Foundry or Azure OpenAI.
2. Enter your Azure API key.
3. Enter the **Endpoint**, **API Version**, and **Deployment / AI model name**.
4. Click **Save**.

Use the full Azure Target URI

For the **Endpoint** field, enter the full Azure Target URI from Azure — not just the base endpoint. Entering only the base endpoint, for example `https://<resource>.openai.azure.com`, prevents credential validation and blocks setup.

Supported formats include:

* `https://<resource>.openai.azure.com/openai/deployments/<deployment>/chat/completions?api-version=<version>`
* `https://<resource>.openai.azure.com/openai/responses?api-version=<version>`

![Example of the Azure AI Foundry integration section](/img/docs/dbt-platform/account-integration-azure-manual.png?v=2 "Example of the Azure AI Foundry integration section")Example of the Azure AI Foundry integration section

To bring your own key instead of using dbt Labs' managed infrastructure, refer to [Configure BYOK for dbt Wizard in dbt platform](./wizard-byok-platform.md).

## Manage spend

dbt Wizard is available to dbt platform Developer, Starter, Enterprise, and Enterprise+ plans. Legacy team plans don't have access to dbt Wizard but can [upgrade](https://www.getdbt.com/pricing) to Starter or Enterprise-tiered plans to access.

Enterprise and Enterprise+ accounts get monthly usage credits, and all other plans start with a 30-day trial with $100 in usage credits. When your credits or trial run out (whichever comes first), set a spend limit so your team can keep working with minimal interruption:

* [Manage your spend limit](../dbt-ai/pricing-billing/trial-and-billing.md#manage-your-spend-limit) to cap what the account can spend on AI usage.
* [Models and pricing](../dbt-ai/pricing-billing/overview.md) explains what each model costs.
* With [BYOK](./wizard-byok-platform.md), your AI provider bills you directly and the usage doesn't draw from your consumption pool.

## Related docs

* [About dbt Wizard in dbt platform](./wizard-platform.md)
* [Configure BYOK in dbt platform](./wizard-byok-platform.md)
* [Trial and billing](../dbt-ai/pricing-billing/trial-and-billing.md)
* [Data & Privacy in dbt platform](../dbt-ai/dbt-ai-faqs.md#privacy-and-data)
