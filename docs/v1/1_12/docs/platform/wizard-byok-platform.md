# Configure BYOK for dbt Wizard in dbt platform

dbt platform

Use bring-your-own-key (BYOK) to connect dbt Wizard or dbt Copilot in dbt platform to your own AI provider account instead of using dbt Labs' managed infrastructure.

The following BYOK instructions apply to dbt platform only. For CLI BYOK setup, refer to [Configure BYOK for dbt Wizard](../dbt-ai/wizard-byok.md).

When you configure a provider with your own key, usage costs appear on your provider account instead of your dbt Labs account, and token costs are billed by whichever provider you choose.

If you'd rather skip that upkeep, the [dbt Labs-managed option](./manage-dbt-ai.md#configure-ai-provider) is ready to use with no setup, and comes with [trial and monthly usage credits](../dbt-ai/pricing-billing/trial-and-billing.md).

## Prerequisites

* A [dbt platform account](https://www.getdbt.com/pricing) on any plan — BYOK is available on Developer, Starter, Enterprise, and Enterprise+ plans
* dbt admin permissions to enable AI features and configure providers in **Account settings**
* An API key or credentials for your supported AI provider

 See the full list of supported AI providers

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

#### dbt Copilot

dbt Copilot supports different AI providers, including bring your own key (BYOK) on any plan:

* dbt Labs-managed OpenAI API key
* BYOK OpenAI API key
* BYOK Azure OpenAI API key

Snowflake Cortex, AWS Bedrock, Azure AI Foundry, and Anthropic aren't supported for dbt Copilot.

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
