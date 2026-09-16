# Wizard Desktop settings [Private beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Local development

Open **Settings** from the bottom left of the app to change how dbt Wizard behaves. Settings are grouped into General, Account, AI providers, Connectors, Appearance, Shortcuts, and version control.

Settings that also exist in the Wizard CLI are shared. Changing them here changes them for `wizard` in your terminal too. Refer to [the config reference](./wizard-config.md) for the underlying files.

Private beta access

Wizard Desktop is in private beta. [Sign up to get an invite](https://www.getdbt.com/wizard-desktop-waitlist), and dbt Labs emails you the download page to get started!

Available on macOS and Linux, with Windows support coming soon.

## General

Controls what appears alongside a chat while you work.

| Setting             | What it does                                                                                                                    |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------- |
| Replay onboarding   | Walks you through first-run setup again. Your existing theme and project settings are kept unless you change them during setup. |
| Application updates | Shows the version you're on, checks for new versions, and installs them.                                                        |

![General settings in Wizard Desktop](/img/wizard/desktop/wizard-desktop-settings-general.png?v=2 "General settings in Wizard Desktop")General settings in Wizard Desktop

## Account

Shows the dbt account dbt Wizard uses for hosted models and platform access, and gives you the tools to clean up what the app has written to disk.

* **Profile** lists the name and email you signed in with, and the dbt platform host the app is connected to, such as `vu491.us1.dbt.com`. Select **Sign out** to disconnect the account.

* **Maintenance** reclaims disk space or returns dbt Wizard to a clean state. This will affect the Wizard CLI too:

  | Action                | What it does                                                                                                                                                                                        |
  | --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
  | Clean up environments | Deletes archived environment directories from disk. The pane tells you whether any are there to remove                                                                                              |
  | Clean up app data     | Drops orphaned records, and shows what the app is tracking, such as `4 environments, 1 project, 4 journal rows`                                                                                     |
  | Reset app             | Permanently deletes your environments, projects, and chat history, plus cached files. It doesn't clear your configuration file at `$DBT_WIZARD_HOME`, which defaults to `~/.dbt/wizard/config.toml` |

Reset app can't be undone

When you select **Reset app**, it can't be undone which means chat history goes with it when you delete it. Make sure you export or open a pull request for any work you want to keep first before taking this action.

## AI providers

Choose how dbt Wizard reaches a model. Come here if you selected **Skip for now** during onboarding, or to switch approaches later. Select **Refresh** to re-check which providers and models are available.

| Option              | What it means                                                                                                                                                                                                                                                                 |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Managed by dbt Labs | No setup. dbt Labs [manages](./pricing-billing/overview.md#dbt-managed-providers) the keys and keeps you on the latest models, and usage is metered against your dbt usage credits. The `dbt` provider shows as **Active** when it's in use |
| Managed by you      | Bring your own key for Amazon Bedrock, Anthropic, Azure OpenAI, Databricks, Google Gemini, OpenAI, or an OpenAI subscription, and more. That provider's API charges apply. Refer to [BYOK](./wizard-byok.md)                                |

Each provider lists how many of its models are enabled, such as `6/6 models enabled`. Select **Manage** to add credentials and turn individual models on or off. You then pick the model and reasoning effort per chat from the chat composer, not here.

## Connectors

*Coming soon*

MCP connectors give dbt Wizard context about your pipelines, BI tools, and orchestration. Browsing and enabling them from this pane is planned for future releases.

Until then, add [MCP servers](./wizard-mcp.md) from the terminal:

1. Start a project chat and send your first message, which starts the terminal.
2. Run `wizard` from the terminal.
3. Run `/mcp` to add a connector or manage the ones you've already added.

## Appearance

Sets how the app looks and how much detail inline widgets show.

| Setting               | What it does                                                                                                                                                                                         |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Theme                 | Switches between **Light**, **Dark**, and **System**. Same choice you made during onboarding. Cycle it with `⌘` + `⌥` + `T`                                                                          |
| Inspector rail labels | Shows the surface names beneath the pane rail icons, so you get **Explorer** and **Lineage** as text rather than icons alone                                                                         |
| Terminal font family  | The font the integrated terminals use. Enter the name exactly as it's installed, for example `FiraCode Nerd Font`. Your preferred family is used first, with `Geist Mono, monospace` as the fallback |
| Terminal font size    | A whole number from 8 to 32                                                                                                                                                                          |

## Shortcuts

Lists the app's keyboard shortcuts, grouped by what they act on.

| Shortcut        | What it does                            |
| --------------- | --------------------------------------- |
| `⌘` + `,`       | Open **Settings**                       |
| `⌘` + `K`       | Open the command palette                |
| `⌘` + `/`       | Show the keyboard shortcuts             |
| `⌘` + `L`       | Focus the chat                          |
| `Esc`           | Close an overlay, or go back to the app |
| `⌘` + `T`       | Open a new chat                         |
| `⌘` + `⇧` + `\` | Toggle the right sidebar                |
| `⌘` + `⌥` + `T` | Cycle the theme                         |

## Version control

*Only GitHub is supported at this time. Support for more version control providers coming soon.*

Set how dbt Wizard authenticates with GitHub to read repos and open pull requests with **Create PR**. Refer to [Review and validate changes](./wizard-desktop-use.md#review-and-validate-changes).

| Method                 | When to use it                                                                                                                                                    |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| GitHub CLI integration | The default. When the [GitHub CLI](https://cli.github.com/) is installed and authenticated, the pane shows a green status dot and the account you're signed in as |
| Personal access token  | Optional, and used when the GitHub CLI isn't available. Select **Create one** to generate a token on GitHub, paste it in, then select **Save**                    |

## Related docs

* [Get started with Wizard Desktop](./wizard-desktop.md)
* [Use Wizard Desktop](./wizard-desktop-use.md)
* [dbt Wizard config reference](./wizard-config.md)
* [Configure BYOK](./wizard-byok.md)
* [Use MCP servers](./wizard-mcp.md)

See it in action and share your feedback

Want to see dbt Wizard in action? Check out the [demo video](https://www.youtube.com/watch?v=-lIzh1xQWMA).

We'd love to hear how dbt Wizard is working for you. Share your feedback by either running the `/feedback` slash command in your interactive terminal session or by going to the [#dbt-wizard](https://getdbt.slack.com/archives/C0B6KLW6T26) channel in the [dbt Community Slack](https://docs.getdbt.com/community/join?version=2.0).

Thanks so much for your help in improving dbt Wizard and dbt data development!
