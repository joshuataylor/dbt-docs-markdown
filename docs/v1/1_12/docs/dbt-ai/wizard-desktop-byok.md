# Configure BYOK in Wizard Desktop [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Local development

Bring your own key (BYOK) means you use credentials from your own AI provider instead of dbt Labs-managed models. Add them in dbt Wizard Desktop under **Settings** → **AI providers**.

With BYOK, dbt Wizard calls your provider directly, so usage costs show up on your provider account, not your dbt account.

Using the CLI or dbt platform instead? Refer to [Configure BYOK for the CLI](./wizard-byok.md) or [Configure BYOK in dbt platform](../platform/wizard-byok-platform.md).

## Add a provider

1. Open **Settings** (`⌘` + `,`) and select **AI providers**.
2. Under **Managed by you**, find your provider and select **Configure**.

   ![AI providers settings in Wizard Desktop](/img/wizard/desktop/wizard-desktop-byok-ai-providers.png?v=2 "AI providers settings in Wizard Desktop")AI providers settings in Wizard Desktop
3. Select the models you want to use or select **Use provider defaults** (if available) to use the recommended selection.
4. Fill in the fields shown for your provider. The app explains each one as you go.
5. Select **Save**.

![Configure a provider, using Amazon Bedrock as an example](/img/wizard/desktop/wizard-desktop-byok-configure-provider.png?v=2 "Configure a provider, using Amazon Bedrock as an example")Configure a provider, using Amazon Bedrock as an example

Each provider shows whether it's set up and how many models are available. You can click **Refresh** if a provider or model you expect isn't listed.

You can pick the model and reasoning effort for each chat from the chat composer.

## Supported providers

dbt Wizard supports [managed models](./pricing-billing/overview.md#dbt-managed-providers) (billed by dbt Labs, no key to manage) and [bring-your-own-key (BYOK)](./wizard-byok.md) models (billed directly by your provider).

Here are the following AI providers supported depending on where you work. Refer to [Model Provider Rate table](https://www.getdbt.com/legal/dbt-wizard-token-costs-by-model) for the full list of available models.

### dbt platform

| Provider                                                                     | Access              |
| ---------------------------------------------------------------------------- | ------------------- |
| [OpenAI](https://openai.com/policies/row-terms-of-use/) (default)            | dbt managed or BYOK |
| [Anthropic](https://www.anthropic.com/legal/consumer-terms)†                 | dbt managed or BYOK |
| Open weight models (like DeepSeek, Kimi, and so on).                         | dbt managed         |
| [Azure AI Foundry](https://www.microsoft.com/licensing/terms) / Azure OpenAI | BYOK                |

### Locally (Desktop and CLI)

Wizard Desktop and the Wizard CLI share the same providers and config.

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

## Update or turn off a provider

You can update or disable a provider directly in Wizard Desktop's **Settings** → **AI providers** page:

* To update credentials: Select **Configure**, enter the new key, and select **Save**. Leave the key blank to keep the saved one.
* To turn a provider off: Select **Configure**, clear every model, and select **Save**.

## Switch the dbt account for managed models

Under **Managed by dbt Labs**, select **Configure** on the `dbt` provider to:

* Choose which dbt account dbt Wizard uses for hosted models and billing. This doesn't change which accounts dbt Wizard can import projects from.
* Turn individual dbt-managed models on or off.

![Configure dbt-managed models and the dbt account](/img/wizard/desktop/wizard-desktop-byok-dbt-managed.png?v=2 "Configure dbt-managed models and the dbt account")Configure dbt-managed models and the dbt account

## Related docs

* [Wizard Desktop settings](./wizard-desktop-settings.md)
* [Get started with Wizard Desktop](./wizard-desktop.md)
* [Configure BYOK for the CLI](./wizard-byok.md)
* [dbt Labs-managed providers and billing](./pricing-billing/overview.md#dbt-managed-providers)

See it in action and share your feedback

Want to see dbt Wizard in action? Check out the [demo video](https://www.youtube.com/watch?v=-lIzh1xQWMA).

We'd love to hear how dbt Wizard is working for you. Share your feedback by either running the `/feedback` slash command in your interactive terminal session or by going to the [#dbt-wizard](https://getdbt.slack.com/archives/C0B6KLW6T26) channel in the [dbt Community Slack](https://docs.getdbt.com/community/join?version=2.0).

Thanks so much for your help in improving dbt Wizard and dbt data development!
