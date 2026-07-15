# Configure BYOK for dbt Wizard [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

You can use the dbt Wizard CLI with bring-your-own-key (BYOK), which means you supply your own credentials from a supported AI provider instead of using dbt Labs' infrastructure.

See it in action and share your feedback

Want to see dbt Wizard in action? Check out the [demo video](https://www.youtube.com/watch?v=-lIzh1xQWMA).

We'd love to hear how dbt Wizard is working for you. Share your feedback by either running the `/feedback` slash command in your interactive terminal session or by going to the [#dbt-wizard](https://getdbt.slack.com/archives/C0B6KLW6T26) channel in the [dbt Community Slack](https://docs.getdbt.com/community/join?version=2.0).

Thanks so much for your help in improving dbt Wizard and dbt data development!

The following BYOK instructions on this page apply to the CLI only. For dbt platform BYOK setup, refer to [Configure BYOK for dbt Wizard in dbt platform](https://docs.getdbt.com/docs/platform/wizard-byok-platform.md).

The "key" in BYOK is whatever credential your chosen provider uses to authenticate API requests — an API key for OpenAI or Anthropic, a bearer token for AWS Bedrock, or a token/PAT for Snowflake Cortex. When you configure a provider with that credential, dbt Wizard calls the provider's API directly using it, so:

* Usage costs appear on your provider account, not your dbt Labs account.
* Token costs are billed by whichever provider you choose.

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

## Configure a provider[​](#configure-a-provider "Direct link to Configure a provider")

You can configure a provider in one of the following ways:

* [**Terminal commands**:](#configure-in-the-terminal) Best for users who want to configure providers from the shell.
* [**Interactive session**:](#configure-in-the-tui) Best for most users working in the dbt Wizard text based user interface (TUI). Use the `/providers` slash command.
* [**Environment variables**:](#set-your-api-key) Best for headless runs, such as `wizard exec`, automation, or temporary local sessions.

### Configure in the terminal[​](#configure-in-the-terminal "Direct link to Configure in the terminal")

In the terminal, use the `providers` subcommand to list, configure, and enable providers:

```bash
wizard providers list
wizard providers configure PROVIDER_NAME
wizard providers enable PROVIDER_NAME
```

Replace `PROVIDER_NAME` with the name of a supported provider, such as `openai`, `anthropic`, `bedrock`, `azure`, `gemini`, `snowflake`, or `databricks`. Then, follow the prompts to enter your credentials.

The `wizard providers list` command shows you the currently configured providers and their status:

```bash
➜  jaffle-shop git:(mwong-fusion) wizard providers list
provider              enabled  route      auth         models
dbt                   true     remote     dbt          3
openai                false    local      missing      3
openai_subscription   false    local      missing      1
anthropic             false    local      missing      3
bedrock               true     local      configured   15
azure                 false    local      missing      3
snowflake             false    local      missing      3
gemini                false    local      missing      3
```

The `configure` command prompts you to enter credentials for the selected provider. To use environment variables or set a key without echoing it in your shell history, refer to [Set your API key](#set-your-api-key).

So for example, if you're using OpenAI, you would run and follow the prompts to configure it:

```bash
wizard providers configure openai
wizard providers enable openai
```

To store an API key without echoing it in your shell history:

```bash
printf '%s' 'sk-...' | wizard providers set-key PROVIDER_NAME
```

Credentials are stored in `~/.dbt/wizard/provider-auth.json`. Provider settings are stored in `~/.dbt/wizard/providers.json`.

### Configure in the TUI[​](#configure-in-the-tui "Direct link to Configure in the TUI")

You can configure providers from an active CLI TUI session with the `/providers` slash command:

```bash
/providers
```

From the provider menu, you can:

* Enable or disable a provider.
* Add or update credentials.
* Select available models.
* Check whether a provider is authenticated and active.

Example provider menu:

```bash
/providers

  Model Providers
  Select a provider to inspect or update it.
 
› 1. dbt                  enabled; 3/3 models selected; uses dbt login; active
  2. openai               disabled; 3/3 models selected; missing credentials; needs setup
  3. openai_subscription  disabled; 1/1 models selected; not connected; needs setup
  4. anthropic            disabled; 3/3 models selected; authenticated; needs setup
  5. bedrock              enabled; 15/15 models selected; authenticated; active
  6. azure                disabled; 3/3 models selected; missing credentials; needs setup
  7. snowflake            disabled; 3/3 models selected; missing credentials; needs setup
  8. gemini               disabled; 3/3 models selected; missing credentials; needs setup
```

## Set your API key[​](#set-your-api-key "Direct link to Set your API key")

* Interactive session
* Environment variable

The first time you start dbt Wizard in a project, onboarding prompts you to choose a provider.

1. At the **Configure a Provider** prompt, select your provider.
2. Paste your API key or provider credentials when prompted.
3. Choose an AI model to finish setup.

To add or switch providers later from an active session, type `/providers` in the TUI.

When you configure a provider through dbt Wizard, credentials are stored in `~/.dbt/wizard/provider-auth.json`.

Set the key as an environment variable if you want to:

* Run dbt Wizard in headless mode, such as with `wizard exec`.
* Use a key for the current terminal [session](https://docs.getdbt.com/docs/dbt-ai/wizard-how-it-works.md#sessions) only.
* Reuse the same key across different terminal sessions.
* Avoid storing credentials in the dbt Wizard config directory.

- OpenAI
- Anthropic
- Amazon Bedrock
- Azure AI Foundry
- Google Gemini
- Snowflake Cortex
- Databricks

```bash
export OPENAI_API_KEY="sk-..."
```

```bash
export ANTHROPIC_API_KEY="sk-ant-..."
```

```bash
export AWS_BEARER_TOKEN_BEDROCK="ABSK..."
```

```bash
export AZURE_API_KEY="..."
```

```bash
export GOOGLE_API_KEY="..."
```

```bash
export SNOWFLAKE_API_KEY="..."
```

```bash
export DATABRICKS_API_KEY="dapi..."
export DATABRICKS_API_BASE="https://adb-1234567890.azuredatabricks.net"
```

To make an environment variable available across terminal sessions, add it to your shell profile, such as `.zshrc`, `.bashrc`, or equivalent.

Avoid committing API keys to version control, project files, or shared configuration.

To persist provider credentials, use one of the following options:

* `wizard providers configure PROVIDER_NAME`
* `wizard providers set-key PROVIDER_NAME`
* The provider's environment variable

## Choose an AI model[​](#choose-an-ai-model "Direct link to Choose an AI model")

You can change the AI model in the following ways:

| Method              | Description                                                                                                                                                     | Example                                                                     |
| ------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------- |
| Interactive session | Change the AI model interactively in the TUI.                                                                                                                   | `/model`                                                                    |
| Invocation flag     | Change the AI model at invocation with the `-m` flag.                                                                                                           | `wizard -m gpt-4o "refactor stg_orders to use incremental materialization"` |
| Config file         | Set a default AI model in the config file (`~/.dbt/wizard/config.toml`); applies to all future sessions. Run `wizard debug models` to list available model IDs. | `model = "gpt-4o"`                                                          |

Restart the dbt Wizard CLI after changing the model.

## Examples[​](#examples "Direct link to Examples")

The following examples use the same provider configuration flow described earlier, with provider-specific credential requirements.

### AWS Bedrock[​](#aws-bedrock "Direct link to AWS Bedrock")

AWS Bedrock is supported in the CLI only. dbt Wizard currently supports Bedrock through an Amazon Bedrock API key, not the full AWS credential chain. Ensure your AWS account has access to the Bedrock models you plan to use and that your Bedrock API key has permission to invoke them.

```bash
export AWS_BEARER_TOKEN_BEDROCK="ABSK..."
wizard providers enable bedrock
wizard providers bedrock set-region us-east-1
wizard providers list
wizard debug models
```

To set a default Bedrock model, add the model ID to `~/.dbt/wizard/config.toml`:

```toml
model = "BEDROCK_MODEL_ID"
```

### Snowflake Cortex [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[​](#snowflake-cortex- "Direct link to snowflake-cortex-")

Snowflake Cortex BYOK support in the CLI is in preview. Availability and setup steps may change. Ensure your Snowflake account has the privileges required for Cortex large language model (LLM) functions. Refer to the [Snowflake Cortex documentation](https://docs.snowflake.com/en/user-guide/snowflake-cortex/overview).

You can [authenticate](#authentication-options) with Snowflake Cortex using an API token or a Programmatic Access Token (PAT) (for SSO and Okta users).

```bash
wizard providers list
wizard providers enable snowflake
wizard providers configure snowflake
wizard providers list
wizard debug models
```

The `wizard providers configure snowflake` command walks you through the following prompts:

```text
Enable this provider? [Y/n]:
Models to enable [1]:
Snowflake account ID:
Snowflake API base override (optional):
Paste API key/token, or press enter to configure it later:
```

| Prompt                                     | What to enter                                                                                              |
| ------------------------------------------ | ---------------------------------------------------------------------------------------------------------- |
| **Snowflake account ID**                   | Your Snowflake account identifier (for example, `myorg-myaccount`)                                         |
| **Snowflake API base override (optional)** | Leave blank — this is only needed for custom or private Snowflake endpoints                                |
| **Paste API key/token**                    | Your authentication token — refer to [Authentication options](#authentication-options) in the next section |

#### Authentication options[​](#authentication-options "Direct link to Authentication options")

The key/token field accepts a regular API token or a Programmatic Access Token (PAT), depending on how your Snowflake account is configured. Both are entered in the same place — the **Paste API key/token** prompt in the terminal, or **Set key/token** (option 3) in the TUI.

1. In your Snowflake account, [generate your API or PAT token](https://docs.snowflake.com/en/user-guide/programmatic-access-tokens#label-pat-generate).
2. Select **Generate token** and copy the token value.
3. Go back to dbt Wizard and paste the PAT at the key/token prompt.

To set a default Snowflake Cortex model, add the model ID to `~/.dbt/wizard/config.toml`:

```toml
model = "SNOWFLAKE_CORTEX_MODEL_ID"
```

### Databricks Unity AI Gateway [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[​](#databricks-unity-ai-gateway- "Direct link to databricks-unity-ai-gateway-")

dbt Wizard connects to Databricks through the [Unity Catalog AI Gateway](https://docs.databricks.com/aws/en/ai-gateway/), so you bring your own models served from your Databricks workspace. Make sure the [serving endpoints](https://docs.databricks.com/en/machine-learning/model-serving/index.html) you plan to use are deployed and that your Databricks token has permission to query them.

```bash
export DATABRICKS_API_KEY="dapi..."
wizard providers configure databricks
wizard providers enable databricks
wizard providers list
wizard debug models
```

The `wizard providers configure databricks` command first asks for your workspace URL, then for the serving endpoint name behind each model you enable:

```text
Enable this provider? [Y/n]:
Models to enable [1]:
Databricks workspace URL (e.g. https://adb-1234567890.azuredatabricks.net):
Endpoint name for databricks/claude-sonnet-4-6:
Paste API key/token, or press enter to configure it later:
```

| Prompt                       | What to enter                                                                                                                                      |
| ---------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Databricks workspace URL** | Your Databricks workspace URL (for example, `https://adb-1234567890.azuredatabricks.net`)                                                          |
| **Endpoint name**            | The serving endpoint name for each model (for example, `databricks-claude-sonnet-4-6`). dbt Wizard suggests a default endpoint name for each model |
| **Paste API key/token**      | Your Databricks personal access token (PAT)                                                                                                        |

By default, dbt Wizard maps the following models to Databricks serving endpoints. You can override any endpoint name during configuration:

| Model               | Default endpoint name          |
| ------------------- | ------------------------------ |
| `claude-sonnet-4-6` | `databricks-claude-sonnet-4-6` |
| `claude-opus-4-7`   | `databricks-claude-opus-4-7`   |
| `claude-haiku-4-5`  | `databricks-claude-haiku-4-5`  |
| `gpt-5.5`           | `databricks-gpt-5-5`           |
| `gpt-5.4`           | `databricks-gpt-5-4`           |
| `gpt-5.4-mini`      | `databricks-gpt-5-4-mini`      |

To set a default Databricks model, add the model ID to `~/.dbt/wizard/config.toml`:

```toml
model = "databricks/claude-sonnet-4-6"
```

## Related docs[​](#related-docs "Direct link to Related docs")

* [Install dbt Wizard](https://docs.getdbt.com/docs/dbt-ai/wizard-cli.md)
* [Configuration reference](https://docs.getdbt.com/docs/dbt-ai/wizard-config.md)
* [dbt Wizard in Studio IDE](https://docs.getdbt.com/docs/dbt-ai/wizard-ide.md)
