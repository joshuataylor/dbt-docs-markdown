# dbt Copilot

dbt platform | Starter, Enterprise, Enterprise+

dbt Copilot helps you generate SQL, documentation, tests, and semantic models in the dbt platform.

dbt Wizard is the recommended agent for dbt work

dbt Wizard is the recommended AI agent for governed data development in dbt. It handles the full development lifecycle — investigation, building, validation, and shipping — grounded in your dbt project's lineage, tests, contracts, and metric definitions.

Refer to [dbt AI FAQs](./dbt-ai-faqs.md#is-dbt-wizard-the-same-as-dbt-copilot), [Billing](./wizard-billing-faqs.md), and [dbt's Terms of Use](https://www.getdbt.com/terms-of-use) for more information.

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

## Configure AI provider for dbt Copilot

You need dbt admin permissions to change account settings and configure providers. dbt Copilot is configured separately from dbt Wizard — for dbt Wizard providers, refer to [Manage AI features](../platform/manage-dbt-ai.md#configure-ai-provider).

dbt Copilot supports fewer providers than dbt Wizard, including bring your own key (BYOK) on any plan:

* dbt Labs-managed OpenAI API key
* BYOK OpenAI API key
* BYOK Azure OpenAI API key

Snowflake Cortex, AWS Bedrock, Azure AI Foundry, and Anthropic aren't supported for dbt Copilot.

dbt Copilot is available for inline assistance in Studio IDE, Canvas, and Insights. Configure it separately from dbt Wizard if your team uses these inline AI experiences.

**To configure a provider:**

1. Click your account name and select **Account settings** in the side menu.
2. Under **Settings**, click **Copilot**.
3. Under **AI providers**, click **Edit** to configure the AI integration.
4. Select your **Key management** option from the dropdown, then follow the steps for your provider below.

### dbt Labs OpenAI

1. Select the toggle for **dbt Labs** to use dbt Labs' managed OpenAI key.
2. Click **Save**.

![Example of the dbt Labs integration page](/img/docs/dbt-platform/account-integration-dbtlabs.png?v=2 "Example of the dbt Labs integration page")Example of the dbt Labs integration page

### OpenAI

1. Select the toggle for **OpenAI** to use your own OpenAI key.
2. Enter the API key.
3. Click **Save**.

![Example of the OpenAI integration page](/img/docs/dbt-platform/account-integration-openai.png?v=2 "Example of the OpenAI integration page")Example of the OpenAI integration page

Data residency limitation

OpenAI projects with [data residency controls](https://platform.openai.com/docs/guides/your-data#data-residency-controls) enabled and configured for the United States (project region set to US) don't currently support BYOK. These projects can only use the API key in the dbt platform configuration. Specifying custom endpoints required for data residency isn’t yet supported, and we’re evaluating a solution for this.

To use BYOK, ensure your OpenAI project doesn’t have data residency controls enabled. Projects without project region settings will use the standard OpenAI endpoint (`https://api.openai.com`) and support BYOK.

### Azure OpenAI

To learn about deploying your own OpenAI model on Azure, refer to [Deploy models on Azure OpenAI](https://learn.microsoft.com/en-us/azure/ai-studio/how-to/deploy-models-openai).

Configure credentials for your Azure OpenAI deployment in dbt as follows:

1. Locate your Azure OpenAI configuration in your Azure Deployment details page.
2. Enter your Azure OpenAI API key.
3. Enter the **Endpoint**, **API Version**, and **Deployment / Model Name**.
4. Click **Save**.

Use the full Azure Target URI

For the **Endpoint** field, enter the full Azure Target URI from Azure — not just the base endpoint. Entering only the base endpoint, for example `https://<resource>.openai.azure.com`, prevents credential validation and blocks setup.

Supported formats include:

* `https://<resource>.openai.azure.com/openai/deployments/<deployment>/chat/completions?api-version=<version>`
* `https://<resource>.openai.azure.com/openai/responses?api-version=<version>`

![Example of Azure OpenAI integration section](/img/docs/dbt-platform/account-integration-azure-manual.png?v=2 "Example of Azure OpenAI integration section")Example of Azure OpenAI integration section

* For BYOK, enable the latest text generation models as well as the `text-embedding-3-small` model.
* Ensure your project doesn't have data residency controls enabled.

## Considerations

* dbt Copilot is a separate experience from dbt Wizard. For agentic, full-lifecycle AI development, use [dbt Wizard](../platform/wizard-overview.md).
* AI features are enabled by default. Admins can [turn them off or back on anytime](../platform/manage-dbt-ai.md).
* Certain features are only available on Enterprise and Enterprise+ plans. Refer to [Billing](../platform/billing.md) for details.
* dbt Copilot doesn't yet support generating semantic models with the latest YAML spec.
