# Install the dbt VS Code extension [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Local developmentⓘ

<!-- -->

The dbt extension — available for [VS Code and Cursor](https://marketplace.visualstudio.com/items?itemName=dbtLabsInc.dbt\&ssr=false#overview), and [Windsurf](https://open-vsx.org/extension/dbtLabsInc/dbt) — uses the dbt Fusion engine to make dbt development smoother and more efficient.

The dbt VS Code extension is compatible with the dbt Fusion engine. It isn't compatible with dbt Core.

note

This is the only official dbt Labs VS Code extension. Other extensions *can* work alongside the dbt VS Code extension, but they aren't tested or supported by dbt Labs. Read the [Fusion Diaries](https://github.com/dbt-labs/dbt-core/discussions/categories/announcements?discussions_q=is:open+diaries+category:Announcements) for the latest updates.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

Before installing, review the [limitations](https://docs.getdbt.com/docs/fusion/supported-features.md#limitations) page because some features don't support Fusion yet.

To use the extension, you need the following:

| Prerequisite                             | Details                                                                                                                                                                                                                                                  |
| ---------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **dbt Fusion engine**                    | The dbt VS Code extension requires the dbt Fusion engine binary. The extension prompts you to install it, or you can [install it manually](#install-the-dbt-fusion-engine-from-the-command-line-if-you-havent-already).                                  |
| **Project files**                        | You need a `profiles.yml` file. You may also need a [`dbt_cloud.yml`](https://docs.getdbt.com/reference/dbt_cloud.yml.md) file for some dbt platform features or credential-based workflows. You don't need a dbt platform project to use the extension. |
| **Editor**                               | [VS Code](https://code.visualstudio.com/), [Cursor](https://www.cursor.com/en), or [Windsurf](https://windsurf.com/editor).                                                                                                                              |
| **Operating system**                     | macOS, Windows, or Linux.                                                                                                                                                                                                                                |
| **Local configuration** (optional)       | [Configure the extension](https://docs.getdbt.com/docs/configure-dbt-extension.md) to mirror your dbt environment locally and set any environment variables required by your project.                                                                    |
| **Project migration support** (optional) | Use the **Migrate dbt Core to Fusion** agent skill or [run dbt-autofix](https://docs.getdbt.com/guides/prepare-fusion-upgrade.md?step=5#what-the-autofix-tool-handles) to help resolve errors and deprecations before using Fusion.                      |

## Install the extension[​](#install-the-extension "Direct link to Install the extension")

1. In your editor, open the **Extensions** tab and search for `dbt`.

2. Locate the extension from the publisher `dbtLabsInc` or `dbt Labs Inc`, then click **Install**.

   [![Search for the extension](/img/docs/extension/extension-marketplace.png?v=2 "Search for the extension")](#)Search for the extension

3. Open a dbt project in your editor. Make sure the project is added to your current workspace.

4. Confirm that the extension is active by checking for the **dbt Extension** label in the status bar. Hover over the label to view diagnostic information.

   [![If you see the 'dbt Extension' label, the extension is activated](/img/docs/extension/dbt-extension-statusbar.png?v=2 "If you see the 'dbt Extension' label, the extension is activated")](#)If you see the 'dbt Extension' label, the extension is activated

5. After the extension activates, it automatically downloads the correct dbt Language Server (LSP) for your operating system.

   [![The dbt Language Server will be installed automatically](/img/docs/extension/extension-lsp-download.png?v=2 "The dbt Language Server will be installed automatically")](#)The dbt Language Server will be installed automatically

6. If the dbt Fusion engine isn't installed, the extension prompts you to download and install it. Follow the notification steps, or [install it manually from the command line](#install-the-dbt-fusion-engine-from-the-command-line-if-you-havent-already).

   [![Follow the prompt to install the dbt Fusion engine](/img/docs/extension/install-dbt-fusion-engine.png?v=2 "Follow the prompt to install the dbt Fusion engine")](#)Follow the prompt to install the dbt Fusion engine

7. Run the VS Code extension [upgrade tool](https://docs.getdbt.com/docs/upgrade-to-fusion-extension.md) to check whether your project is ready for Fusion and fix any errors or deprecations.

8. Optional: [Configure your local environment](https://docs.getdbt.com/docs/configure-dbt-extension.md) to mirror your dbt platform environment and [set environment variables](https://docs.getdbt.com/docs/configure-dbt-extension.md#configure-environment-variables) required by your project.

You're ready to use the dbt extension. Next, you can:

* Follow the [getting started](#getting-started) workflow to finish setup.
* [Upgrade your project to Fusion](https://docs.getdbt.com/docs/upgrade-to-fusion-extension.md) if you're migrating from dbt Core.
* [Sign in or register](https://docs.getdbt.com/docs/sign-in-dbt-extension.md) for a dbt platform account to keep using advanced features after the 14-day trial.
* Review the [limitations and unsupported features](https://docs.getdbt.com/docs/fusion/supported-features.md#limitations).

 Install the dbt Fusion engine from the command line, if you haven't already.

Choose your preferred installation method:

 Pip installation for Windows, macOS, and Linux

```shell
python -m pip install --pre dbt
```

To upgrade to a newer version:

```shell
python -m pip install --upgrade --pre dbt
```

 Standalone installation for macOS and Linux

```shell
curl -fsSL https://public.cdn.getdbt.com/fs/install/install.sh | sh -s -- --update
```

To use `dbt` immediately after installation, close and reopen your terminal or reload your shell so that the new `$PATH` is recognized:

```shell
exec $SHELL
```

To upgrade to a newer version:

```shell
dbt system update
```

note

`dbtf system update` installs Fusion globally — it updates your `PATH` in `~/.zshrc` and creates a `dbtf` alias. To manage multiple versions or isolate your install, use separate shell profiles or virtual environments.

 Standalone installation for Windows

```powershell
irm https://public.cdn.getdbt.com/fs/install/install.ps1 | iex
```

To use `dbt` immediately after installation, close and reopen or reload your shell so that the new `Path` is recognized:

```powershell
Start-Process powershell
```

To upgrade to a newer version:

```shell
dbt system update
```

 Homebrew installation for macOS

```shell
brew tap dbt-labs/dbt
brew install dbt
```

To upgrade to a newer version:

```shell
brew tap dbt-labs/dbt
brew upgrade dbt
```

 Winget installation for Windows

```shell
winget install --id dbtLabs.dbt --exact
```

To upgrade to a specific version:

```shell
winget install --id dbtLabs.dbt --exact --version <version>
```

Run the following command to verify your installation:

```bash
dbt --version
```

You can use `dbt` or its Fusion alias `dbtf` (handy if you already have the Core or platform CLI installed). Default install path:

* macOS/Linux: `$HOME/.local/bin/dbt`
* Windows: `C:\Users\<username>\.local\bin\dbt.exe`

The installer adds this path automatically, but you may need to reload your shell for the `dbtf` command to work.

After installation, follow the [getting started](#getting-started) workflow. You can get started by:

* Running `dbt init --fusion-upgrade` to start terminal onboarding.
* Running **dbt: Register dbt extension** from the command palette.
* Selecting **Get started** from the extension menu.

## Getting started[​](#getting-started "Direct link to Getting started")

After the dbt Fusion engine and dbt VS Code extension are installed, the dbt logo appears in the sidebar. Click it to open the **Get started** panel.

The **Get started** panel is a persistent setup companion available in VS Code and Cursor. It monitors your environment and shows the most important next action. As new Fusion releases ship or your project changes, the panel resurfaces relevant steps automatically. For more details refer to [Upgrade to Fusion](https://docs.getdbt.com/docs/upgrade-to-fusion-extension.md).

[![The Get started panel in VS Code showing the setup steps, with the active step highlighted.](/img/docs/extension/vsce-get-started.png?v=2 "The Get started panel in VS Code showing the setup steps, with the active step highlighted.")](#)The Get started panel in VS Code showing the setup steps, with the active step highlighted.

The panel guides you through:

1. **Install or update dbt Fusion engine**: Detects whether the dbt Fusion engine binary is missing or outdated and installs or updates it with a single click.
2. **Open project**: Checks for a `dbt_project.yml` file in your workspace to confirm a valid dbt project is open before proceeding.
3. **Check Fusion compatibility**: Guides you through upgrading your project to Fusion. You can choose between an agentic migration or manual CLI onboarding — refer to [Upgrade to Fusion](https://docs.getdbt.com/docs/upgrade-to-fusion-extension.md).
4. **Register**: Confirms you've registered your email to use the extension beyond the 14-day trial period — refer to [Sign in or register](https://docs.getdbt.com/docs/sign-in-dbt-extension.md).

When all setup steps are complete, the panel shows a green **Extension setup complete** button.

[![The Get started panel showing Extension setup complete with all four steps checked.](/img/docs/extension/vsce-get-started-complete.png?v=2 "The Get started panel showing Extension setup complete with all four steps checked.")](#)The Get started panel showing Extension setup complete with all four steps checked.

## Next steps[​](#next-steps "Direct link to Next steps")

Once you've installed the dbt VS Code extension, go to the next pages to get started:

1. Review the [Upgrade to Fusion](https://docs.getdbt.com/docs/upgrade-to-fusion-extension.md) page to ensure your dbt project is using Fusion.
2. [Sign in or register](https://docs.getdbt.com/docs/sign-in-dbt-extension.md) for a free dbt platform account to keep using advanced features after the 14-day trial.
3. Review the [limitations and unsupported features](https://docs.getdbt.com/docs/fusion/supported-features.md#limitations).
