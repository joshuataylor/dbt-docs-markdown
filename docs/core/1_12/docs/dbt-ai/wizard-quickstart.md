# Get started with the dbt Wizard local CLI

Install dbt Wizard locally and start an agentic dbt development session from your terminal.

You can run the dbt Wizard CLI locally from any dbt project that uses the dbt CLI, Fusion, or dbt Core.

Install dbt Wizard as `wizard` on your `PATH` using the curl script for your operating system:

* macOS/Linux
* Windows

```bash
curl -fsSL https://public.cdn.getdbt.com/dbt-wizard/install/install-wizard.sh | sh
```

Run the following in PowerShell:

```powershell
irm https://public.cdn.getdbt.com/dbt-wizard/install/install-wizard.ps1 | iex
```

Then verify the install and start a session:

```bash
wizard --version   # confirm the install
wizard             # start an interactive session
```

After running `wizard --version`, you should see something like `dbt-wizard VERSION`. Run `wizard --help` to see all available commands and flags. dbt Wizard installs default config files — refer to the [config reference](https://docs.getdbt.com/docs/dbt-ai/wizard-config.md) for more details.

Upgrade for automatic updates

Upgrade to dbt Fusion engine or dbt Core v2.0 or later to run dbt Wizard as `wizard` and get automatic updates.

By the end of this guide, you can install dbt Wizard locally, authenticate with your dbt platform credentials if applicable, complete first-run onboarding, and send your first prompt from the terminal.

dbt Wizard is data warehouse agnostic and works with both the [dbt Fusion engine](https://docs.getdbt.com/docs/fusion.md) and [dbt Core](https://docs.getdbt.com/docs/local/install-dbt.md) — no specific engine is required.

Be warned, the wizard has been known to cast spells

.

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

Upgrade to the dbt Fusion engine

On dbt Fusion engine (version 2.0 and later), start dbt Wizard with `wizard` and use `wizard COMMAND_NAME` for CLI commands.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

You'll need:

* An OpenAI subscription, or your own API key or provider credentials for a supported provider using [BYOK](https://docs.getdbt.com/docs/dbt-ai/wizard-byok.md): OpenAI, Anthropic, AWS Bedrock, Azure, Snowflake Cortex (preview), or Databricks
* A dbt project with a built `target/` directory (run `dbt parse`, `dbt compile`, or `dbt build`)

New to the terminal?

If you've never used the terminal before, check out the [terminal guide](https://docs.getdbt.com/guides/terminal-guide.md). Some tips include:

* Enter `/` to see the available commands or try out `/overview` to get a quick summary of your project.
* Press `Shift+Tab` to cycle through collaboration modes.

## Complete first-run onboarding[​](#complete-first-run-onboarding "Direct link to Complete first-run onboarding")

The first time you start dbt Wizard in a project, it walks you through a short setup and saves your answers to [`wizard_config.toml`, `providers.json`, and `provider-auth.json`](https://docs.getdbt.com/docs/dbt-ai/wizard-config.md), so you only do this once per project. You'll be asked to:

* Review and accept the **Terms of Use**.
* If you have a dbt platform account, sign in through the browser authentication link when prompted and follow the steps in the browser.
* **Trust the directory** so dbt Wizard can read your project.
* Confirm the **path to your dbt executable or virtual environment** — point it at `/path/to/bin/dbt` or a `.venv` root (dbt Wizard uses `bin/dbt` automatically).
* Add any **extra compile flags** to append to the startup `dbt compile -s state:modified+`, or leave empty to skip.
* **Configure [deferral](https://docs.getdbt.com/docs/dbt-ai/wizard-config.md#deferral)** — choose **Wizard** (dbt Wizard manages it), **Manual**, or **Disabled**. If you choose **Wizard**, enter the `profiles.yml` target to defer to (defaults to `prod`). On the dbt Fusion engine connected to the dbt platform, dbt Wizard instead offers to let the platform handle deferral.
* Confirm your **detected dbt profile and target**, or customize the profile, target, or `profiles.yml` path.
* **Configure a provider** (OpenAI subscription, OpenAI API key, Anthropic, Amazon Bedrock, Azure, Gemini, or Snowflake). Refer to [Configure BYOK](https://docs.getdbt.com/docs/dbt-ai/wizard-byok.md).

To re-run any of these steps later, refer to [Re-trigger onboarding flows](https://docs.getdbt.com/docs/dbt-ai/wizard-config.md#re-trigger-onboarding-flows).

Set your API key without the prompt

During onboarding, dbt Wizard prompts you to configure a provider interactively. To skip the API key prompt — for headless runs like `wizard exec` or to reuse your key across sessions — set it as an environment variable before starting `wizard` instead:

```bash
export OPENAI_API_KEY="sk-..."                  # or ANTHROPIC_API_KEY
export AWS_BEARER_TOKEN_BEDROCK="ABSK..."       # for Amazon Bedrock
```

For AWS Bedrock and Snowflake Cortex, refer to [Configure BYOK](https://docs.getdbt.com/docs/dbt-ai/wizard-byok.md).

After onboarding, dbt Wizard shows a welcome screen with two sections:

* **STATUS** — the dbt Wizard version, the active AI model (change it with `/model`), and your project directory.
* **OVERVIEW** — a snapshot of your project from the [metadata engine](https://docs.getdbt.com/docs/dbt-ai/wizard-how-it-works.md#native-metadata-engine): build status and a count of passing (✓), warning (⚠), and failing (✗) checks.

Enter `/` to see the available [slash commands](https://docs.getdbt.com/docs/dbt-ai/wizard-slash-commands.md), or try `/overview` for a summary of your project. CLI commands use the `wizard` prefix, so you can also run [subcommands](https://docs.getdbt.com/docs/dbt-ai/wizard-cli-reference.md) such as `wizard exec`, `wizard review`, and `wizard resume`.

Once you're set up, ask your first question in your terminal. Try some [prompts](https://docs.getdbt.com/docs/dbt-ai/wizard-use-cases.md) to see how dbt Wizard works:

**Understand your project:**

```text
summarize what this project does
```

**Find coverage gaps:**

```text
which models in this project have no tests?
```

**Enhance coverage:**

```text
add not_null and unique tests to the primary key of stg_customers
```

**Ship changes:**

```text
commit the changes to the branch and create a pull request
```

dbt Wizard will read your project's lineage, tests, and metadata and propose changes as a diff. You approve, reject, or redirect before anything is written.

Your browser does not support the video tag.

dbt Wizard CLI in your terminal

For refactor or change requests, dbt Wizard automatically assesses downstream impact first by reporting affected models, metrics, and tests with a severity rating before proposing any changes.

When dbt Wizard manages deferral, you point it at a target in your `profiles.yml` and it compiles and defers to that target automatically, so it can validate against already-built upstream models without rebuilding everything. Refer to [Deferral and state](https://docs.getdbt.com/docs/dbt-ai/wizard-how-it-works.md#deferral-and-state) and [About dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md) for details.

## Useful terminal commands[​](#useful-terminal-commands "Direct link to Useful terminal commands")

Use the following commands to get started:

| Command                       | Description                                                                                                                     | Example                                       |
| ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------- |
| `wizard "[prompt]"`           | Start an interactive session seeded with a prompt. Once you activate the session, you don't need to pass your prompt in quotes. | `wizard "summarize what this project does"`   |
| `wizard exec "[prompt]"`      | Run a single prompt non-interactively and exit                                                                                  | `wizard exec "list all models with no tests"` |
| `wizard review --uncommitted` | Non-interactive code review of uncommitted changes                                                                              | `wizard review --uncommitted`                 |
| `wizard review --base BRANCH` | Review diff against a base branch                                                                                               | `wizard review --base main`                   |
| `wizard resume`               | Resume a previous session                                                                                                       | `wizard resume --last`                        |
| `wizard apply`                | Apply the latest Wizard diff to your working directory                                                                          | `wizard apply TASK_ID`                        |
| `wizard login` / `logout`     | Authenticate with your dbt platform account                                                                                     | `wizard login`                                |
| `wizard mcp`                  | Manage MCP server connections                                                                                                   | `wizard mcp add dbt`                          |
| `wizard update`               | Update Wizard to the latest version                                                                                             | `wizard update`                               |

Need to re-run setup?

If you want to re-run onboarding — re-authenticate, reset project config, or retrigger the trusted folder prompt — refer to [Re-trigger onboarding flows](https://docs.getdbt.com/docs/dbt-ai/wizard-config.md#re-trigger-onboarding-flows).

## Next steps[​](#next-steps "Direct link to Next steps")

* [Use cases and examples](https://docs.getdbt.com/docs/dbt-ai/wizard-use-cases.md) for realistic analytics engineering scenarios
* [Install and update reference](https://docs.getdbt.com/docs/dbt-ai/wizard-cli.md) for full install, update, and uninstall details
* [Configure BYOK](https://docs.getdbt.com/docs/dbt-ai/wizard-byok.md) for managing your API key and choosing an AI model
* [Configuration reference](https://docs.getdbt.com/docs/dbt-ai/wizard-config.md) for setting persistent defaults in `config.toml` and per-project dbt settings in `wizard_config.toml`
* [Use skills locally](https://docs.getdbt.com/docs/dbt-ai/wizard-skills.md) for giving Wizard reusable instructions for your project
* [Use MCP servers](https://docs.getdbt.com/docs/dbt-ai/wizard-mcp.md) to connect dbt Wizard CLI to more tools and context
* [Migrate from Claude Code](https://docs.getdbt.com/docs/dbt-ai/wizard-migrate.md) for bringing existing Claude Code project context into dbt Wizard

See it in action and share your feedback

Want to see dbt Wizard in action? Check out the [demo video](https://www.youtube.com/watch?v=-lIzh1xQWMA).

We'd love to hear how dbt Wizard is working for you. Share your feedback by either running the `/feedback` slash command in your interactive terminal session or by going to the [#dbt-wizard](https://getdbt.slack.com/archives/C0B6KLW6T26) channel in the [dbt Community Slack](https://docs.getdbt.com/community/join?version=2.0).

Thanks so much for your help in improving dbt Wizard and dbt data development!
