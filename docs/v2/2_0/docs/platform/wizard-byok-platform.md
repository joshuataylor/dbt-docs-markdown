# Configure BYOK for dbt Wizard in dbt platform

Use bring-your-own-key (BYOK) to connect dbt Wizard or dbt Copilot in dbt platform to your own AI provider account instead of using dbt Labs' managed infrastructure.

The following BYOK instructions apply to dbt platform only. For CLI BYOK setup, refer to [Configure BYOK for dbt Wizard](https://docs.getdbt.com/docs/dbt-ai/wizard-byok.md).

When you configure a provider with your own key, usage costs appear on your provider account instead of your dbt Labs account, and token costs are billed by whichever provider you choose.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* A [dbt platform account](https://www.getdbt.com/pricing) on Starter, Enterprise, or Enterprise+ plans
* dbt admin permissions to enable AI features and configure providers in **Account settings**
* AI features enabled for your account — refer to [Enable AI in dbt platform](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md#enable-ai-features)
* An API key or credentials for your supported AI provider

 See the full list of supported AI providers

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

#### dbt Copilot[​](#dbt-copilot "Direct link to dbt Copilot")

dbt Copilot supports different AI providers, including bring your own key (BYOK) for Enterprise and Enterprise+ plans:

* dbt Labs-managed OpenAI API key
* BYOK OpenAI API key
* BYOK Azure OpenAI API key

Snowflake Cortex, AWS Bedrock, Azure AI Foundry, and Anthropic aren't supported for dbt Copilot.

## Configure AI provider[​](#configure-ai-provider "Direct link to Configure AI provider")

Once AI features have been [enabled](https://docs.getdbt.com/docs/platform/enable-dbt-ai.md#enable-ai-features), Enterprise and Enterprise+ accounts can configure a custom AI provider. If you bring your own provider, you will incur API calls and associated charges from that provider.

\* *Managed (or Managed by dbt Labs): dbt Labs manages the AI provider connection; no user provider key is required. Refer to [Billing](https://docs.getdbt.com/docs/platform/billing.md?version=2.0\&name=Fusion#temporary-dbt-copilot-actions-bridge-through-july-1) for more information.*

### dbt Wizard[​](#dbt-wizard "Direct link to dbt Wizard")

To configure your AI provider for dbt Wizard:

1. Click on your account name and select **Account settings** in the side menu.
2. Under **Settings**, click **AI features**.
3. Under **AI providers**, click **Edit** to configure the AI integration.
4. For each provider, select your **Key management** option from the dropdown.

* OpenAI
* Azure AI Foundry
* Anthropic

**Managed by dbt Labs** (default, no setup required). Refer to [Billing](https://docs.getdbt.com/docs/platform/billing.md?version=2.0\&name=Fusion#temporary-dbt-copilot-actions-bridge-through-july-1) for more information.\*

1. Select the toggle for **dbt Labs** to use dbt Labs' managed\* OpenAI key.
2. Click **Save**.

[![Example of the dbt Labs integration page](/img/docs/dbt-platform/account-integration-dbtlabs.png?v=2 "Example of the dbt Labs integration page")](#)Example of the dbt Labs integration page

**Managed by you** (Enterprise or Enterprise+ plans)

1. Select **OpenAI** from the options.
2. Enter your OpenAI API key.
3. Click **Save**.

[![Example of the OpenAI integration page](/img/docs/dbt-platform/account-integration-openai.png?v=2 "Example of the OpenAI integration page")](#)Example of the OpenAI integration page

Data residency limitation

OpenAI projects with [data residency controls](https://platform.openai.com/docs/guides/your-data#data-residency-controls) enabled and configured for the United States (project region set to US) don't currently support BYOK. These projects can only use the API key in the dbt platform configuration. Specifying custom endpoints required for data residency isn’t yet supported, and we’re evaluating a solution for this.

To use BYOK, ensure your OpenAI project doesn’t have data residency controls enabled. Projects without project region settings will use the standard OpenAI endpoint (`https://api.openai.com`) and support BYOK.

* For BYOK, enable the latest text generation models as well as the `text-embedding-3-small` model.
* Ensure your project doesn't have data residency controls enabled. Projects without project region settings use the standard OpenAI endpoint (<https://api.openai.com>) and support BYOK.

**Managed by you only** (Enterprise or Enterprise+ plans)

To learn about deploying models on Azure, refer to [Deploy models on Azure OpenAI](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/deploy-models-openai) and [Azure AI Foundry](https://learn.microsoft.com/en-us/azure/ai-foundry/). Configure credentials for your Azure AI Foundry deployment in dbt as follows:

1. Locate your deployment details in the Azure portal (Azure AI Foundry or Azure OpenAI).
2. Enter your Azure API key.
3. Enter the **Endpoint**, **API Version**, and **Deployment / AI model name**.
4. Click **Save**.

Use the full Azure Target URI

For the **Endpoint** field, enter the full Azure Target URI from Azure — not just the base endpoint. Entering only the base endpoint (for example, `https://<resource>.openai.azure.com`) prevents credential validation and blocks setup.

Supported formats include:

* `https://<resource>.openai.azure.com/openai/deployments/<deployment>/chat/completions?api-version=<version>`
* `https://<resource>.openai.azure.com/openai/responses?api-version=<version>`

[![Example of the Azure AI Foundry integration section](/img/docs/dbt-platform/account-integration-azure-manual.png?v=2 "Example of the Azure AI Foundry integration section")](#)Example of the Azure AI Foundry integration section

**Managed by dbt Labs** (default, no setup required). Refer to [Billing](https://docs.getdbt.com/docs/platform/billing.md?version=2.0\&name=Fusion#temporary-dbt-copilot-actions-bridge-through-july-1) for more information.\*

1. Select **dbt Labs** from the list to use dbt Labs' managed\* Anthropic key.
2. Click **Save**.

**Managed by you** (Enterprise or Enterprise+ plans)

1. Select **Anthropic** from the options.
2. Enter your Anthropic API key.
3. Click **Save**.

Embedding limitations

When using an Anthropic API key, dbt continues to use the dbt Labs-managed OpenAI key for embeddings in `text_to_sql` MCP tools, since Anthropic doesn't natively provide embeddings.

### dbt Copilot[​](#dbt-copilot "Direct link to dbt Copilot")

To configure your AI provider for dbt Copilot:

1. Click on your account name and select **Account settings** in the side menu.
2. Under **Settings**, click **Copilot**.
3. Under **AI providers**, click **Edit** to configure the AI integration.
4. For each provider, select your **Key management** option from the dropdown.

* dbt Labs OpenAI
* OpenAI
* Azure OpenAI

1. Select the toggle for **dbt Labs** to use dbt Labs' managed\* OpenAI key.
2. Click **Save**.

[![Example of the dbt Labs integration page](/img/docs/dbt-platform/account-integration-dbtlabs.png?v=2 "Example of the dbt Labs integration page")](#)Example of the dbt Labs integration page

Bringing your own OpenAI key is available for Enterprise or Enterprise+ plans.

1. Select the toggle for **OpenAI** to use your own OpenAI key.
2. Enter the API key.
3. Click **Save**.

[![Example of the OpenAI integration page](/img/docs/dbt-platform/account-integration-openai.png?v=2 "Example of the OpenAI integration page")](#)Example of the OpenAI integration page

Data residency limitation

OpenAI projects with [data residency controls](https://platform.openai.com/docs/guides/your-data#data-residency-controls) enabled and configured for the United States (project region set to US) don't currently support BYOK. These projects can only use the API key in the dbt platform configuration. Specifying custom endpoints required for data residency isn’t yet supported, and we’re evaluating a solution for this.

To use BYOK, ensure your OpenAI project doesn’t have data residency controls enabled. Projects without project region settings will use the standard OpenAI endpoint (`https://api.openai.com`) and support BYOK.

Bringing your own Azure OpenAI key is available for Enterprise or Enterprise+ plans.

To learn about deploying your own OpenAI model on Azure, refer to [Deploy models on Azure OpenAI](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/deploy-models-openai). Configure credentials for your Azure OpenAI deployment in dbt as follows:

1. Locate your Azure OpenAI configuration in your Azure Deployment details page.
2. Enter your Azure OpenAI API key.
3. Enter the **Endpoint**, **API Version**, and **Deployment / Model Name**.
4. Click **Save**.

Use the full Azure Target URI

For the **Endpoint** field, enter the full Azure Target URI from Azure — not just the base endpoint. Entering only the base endpoint (for example, `https://<resource>.openai.azure.com`) prevents credential validation and blocks setup.

Supported formats include:

* `https://<resource>.openai.azure.com/openai/deployments/<deployment>/chat/completions?api-version=<version>`
* `https://<resource>.openai.azure.com/openai/responses?api-version=<version>`

[![Example of Azure OpenAI integration section](/img/docs/dbt-platform/account-integration-azure-manual.png?v=2 "Example of Azure OpenAI integration section")](#)Example of Azure OpenAI integration section

* For BYOK, enable the latest text generation models as well as the `text-embedding-3-small` model.
* Ensure your project doesn't have data residency controls enabled.

*dbt Wizard is available in Studio IDE as a public preview feature, and as a standalone beta feature across the managed dbt platform. However, certain customers may have disabled experimental features, in which case, they can use Wizard CLI via terminal-access and will continue to have access to dbt Copilot until dbt Wizard is released as generally available to all customers. [Contact dbt Support](mailto:support@getdbt.com) with any questions.*

Please contact dbt Support with any questions
