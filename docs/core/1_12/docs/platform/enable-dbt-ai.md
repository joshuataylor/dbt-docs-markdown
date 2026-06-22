# Get started in dbt platform [Starter](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")

Enable AI features in dbt platform to speed up your development and focus on delivering quality data.

You can control AI features in dbt platform for dbt Wizard and dbt Copilot in your **Account settings**.

* **dbt Wizard**: dbt's innovative and recommended agentic AI layer, available on the dbt platform and CLI.
* **dbt Copilot**: Inline AI assistance (code generation, docs, tests, metrics buttons) in Studio IDE, as well as Canvas and Insights.

Both experiences are controlled by a single AI toggle in **Account settings**.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* Must have a [on Starter, Enterprise, or Enterprise+ plans](https://www.getdbt.com/pricing).
  <!-- -->
  * Certain features like [natural prompts in Canvas](https://docs.getdbt.com/docs/platform/build-canvas-copilot.md) are only available on Enterprise and Enterprise+ plans.
* Development environment is on a supported [release track](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md) to receive ongoing updates.
* Opt-in to AI features by following the steps in the next section in your **Account settings**.

## Enable AI features[​](#enable-ai-features "Direct link to Enable AI features")

To opt in to AI features, a dbt admin can follow these steps:

1. Navigate to **Account settings** in the navigation menu.
2. Under **Settings**, confirm the account you're enabling.
3. Click **Edit** in the top right corner.
4. Enable the **Enable account access to AI features** option.
5. Click **Save**. You should now have AI features enabled for use.

Note: To disable (only after enabled), repeat steps 1 to 3, toggle off in step 4, and repeat step 5.

## Try your first prompt[​](#try-your-first-prompt "Direct link to Try your first prompt")

After AI features are enabled, open dbt Wizard from the left sidebar in the dbt platform. You can use it from the home tab for an agent-native workflow, or from Studio IDE when you want to work alongside the file editor.

Try a prompt such as:

* `summarize what this project does`
* `which models in this project have no tests?`
* `add not_null and unique tests to the primary key of stg_customers`

Use the home tab to investigate, generate, review diffs, and run validations. Use Studio IDE when you want direct file control with the editor, console, and file explorer.

## Next steps[​](#next-steps "Direct link to Next steps")

* [dbt Wizard home tab](https://docs.getdbt.com/docs/platform/wizard-home.md)
* [dbt Wizard in Studio IDE](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md)
* [Prompt cookbook](https://docs.getdbt.com/guides/prompt-cookbook.md)

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

#### dbt Copilot[​](#dbt-copilot "Direct link to dbt Copilot")

dbt Copilot supports different AI providers, including bring your own key (BYOK) for Enterprise and Enterprise+ plans:

* dbt Labs-managed OpenAI API key
* BYOK OpenAI API key
* BYOK Azure OpenAI API key

Snowflake Cortex, AWS Bedrock, Azure AI Foundry, and Anthropic aren't supported for dbt Copilot.

 What's the difference between dbt Wizard and dbt Copilot?

info

dbt Wizard is the new and recommended AI agent for governed data development in dbt. It handles the full development lifecycle — investigation, building, validation, and shipping — grounded in your dbt project's lineage, tests, contracts, and metric definitions.

dbt Copilot is separate from dbt Wizard and is dbt's inline AI assistance experience, providing single-click generation of SQL, documentation, tests, and semantic models in Studio IDE, Canvas, and Insights.

Refer to [dbt AI FAQs](https://docs.getdbt.com/docs/dbt-ai/dbt-ai-faqs.md#is-dbt-wizard-the-same-as-dbt-copilot), [Billing](https://docs.getdbt.com/docs/platform/billing.md), and [dbt's Terms of Use](https://www.getdbt.com/terms-of-use) for more information.

## Configure AI provider [Enterprise](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[Enterprise +](https://www.getdbt.com/pricing "Go to https://www.getdbt.com/pricing")[​](#configure-ai-provider- "Direct link to configure-ai-provider-")

Once AI features have been [enabled](#enable-ai-features), Enterprise and Enterprise+ accounts can configure a custom AI provider. If you bring your own provider, you will incur API calls and associated charges from that provider.

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

**Managed\* by dbt Labs** (default, no setup required). Refer to [Billing](https://docs.getdbt.com/docs/platform/billing.md?version=2.0\&name=Fusion#temporary-dbt-copilot-actions-bridge-through-july-1) for more information.\*

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

**Managed\* by dbt Labs** (default, no setup required). Refer to [Billing](https://docs.getdbt.com/docs/platform/billing.md?version=2.0\&name=Fusion#temporary-dbt-copilot-actions-bridge-through-july-1) for more information.\*

1. Select **dbt Labs** from the list to use dbt Labs' managed\* Anthropic key.
2. Click **Save**.

**Managed by you** (Enterprise or Enterprise+ plans)

1. Select **Anthropic** from the options.
2. Enter your Anthropic API key.
3. Click **Save**.

Embedding limitations

When using an Anthropic API key, dbt continues to use the dbt Labs-managed OpenAI key for embeddings in `text_to_sql` MCP tools, since Anthropic doesn't natively provide embeddings.

### dbt Copilot[​](#dbt-copilot-1 "Direct link to dbt Copilot")

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

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
