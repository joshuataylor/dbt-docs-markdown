# Install the dbt VS Code extension [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Local development

The dbt extension — available for [VS Code and Cursor](https://marketplace.visualstudio.com/items?itemName=dbtLabsInc.dbt\&ssr=false#overview), and [Windsurf](https://open-vsx.org/extension/dbtLabsInc/dbt) — makes dbt development smoother and more efficient. dbt v1 and v2 both support the extension — refer to [Version compatibility](./about-dbt-extension.md#version-compatibility) for which features need which setup.

note

This is the only official dbt Labs VS Code extension. Other extensions *can* work alongside the dbt VS Code extension, but they aren't tested or supported by dbt Labs.

## Prerequisites

To use the extension, you need the following:

| Prerequisite                             | Details                                                                                                                                                                                                                                                                                                                                                        |
| ---------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Project files**                        | You need a `profiles.yml` file. You may also need a [`dbt_cloud.yml`](../reference/dbt_cloud.yml.md) file for some dbt platform features or credential-based workflows. You don't need a dbt platform project to use the extension.                                                                                                       |
| **Editor**                               | [VS Code](https://code.visualstudio.com/), [Cursor](https://www.cursor.com/en), or [Windsurf](https://windsurf.com/editor).                                                                                                                                                                                                                                    |
| **Operating system**                     | macOS, Windows, or Linux.                                                                                                                                                                                                                                                                                                                                      |
| **Local configuration** (optional)       | [Configure the extension](./configure-dbt-extension.md) to mirror your dbt environment locally and set any environment variables required by your project.                                                                                                                                                                          |
| **Project migration support** (optional) | The extension has [dbt-autofix](https://github.com/dbt-labs/dbt-autofix) built in, so you can fix deprecations from the **Problems** pane or in a single pull request. You can also use the **Migrate dbt v1 to dbt v2** agent skill or [run dbt-autofix](../guides/prepare-v2-upgrade.md?step=5#what-the-autofix-tool-handles) yourself. |

## Install the extension

1. In your editor, open the **Extensions** tab and search for `dbt`.

2. Locate the extension from the publisher `dbtLabsInc` or `dbt Labs Inc`, then click **Install**.

   ![Search for the extension](/img/docs/extension/extension-marketplace.png?v=2 "Search for the extension")Search for the extension

3. Open a dbt project in your editor. Make sure the project is added to your current workspace.

4. Confirm that the extension is active by checking for the **dbt Extension** label in the status bar. Hover over the label to view diagnostic information.

   ![If you see the 'dbt Extension' label, the extension is activated](/img/docs/extension/dbt-extension-statusbar.png?v=2 "If you see the 'dbt Extension' label, the extension is activated")If you see the 'dbt Extension' label, the extension is activated

5. After the extension activates, it automatically downloads the correct dbt Language Server (LSP) for your operating system.

   ![The dbt Language Server will be installed automatically](/img/docs/extension/extension-lsp-download.png?v=2 "The dbt Language Server will be installed automatically")The dbt Language Server will be installed automatically

6. If dbt isn't installed, the extension prompts you to download and install it. Follow the notification steps, or [install it manually from the command line](#install-dbt-v2-from-the-command-line-if-you-havent-already).

   ![Follow the prompt to install v2](/img/docs/extension/install-dbt-fusion-engine.png?v=2 "Follow the prompt to install v2")Follow the prompt to install v2

7. Run the VS Code extension [upgrade tool](./upgrade-to-dbt-extension.md) to check whether your project is ready for dbt v2 and fix any errors or deprecations.

8. Optional: [Configure your local environment](./configure-dbt-extension.md) to mirror your dbt platform environment and [set environment variables](./configure-dbt-extension.md#configure-environment-variables) required by your project.

The language server ships with dbt v2

The dbt language server is part of the dbt v2 binary rather than a separate download — the extension runs it through dbt v2. If you install v2 manually (for example, in an air-gapped environment) instead of letting the extension manage it, use the [version compatibility matrix](./dbt-versions/dbt-version-compatibility.md) to pick a binary that matches your extension version.

You're ready to use the dbt extension. Next, you can:

* Follow the [getting started](#getting-started) workflow to finish setup.
* [Upgrade your project to dbt v2](./upgrade-to-dbt-extension.md) if you're migrating from dbt v1.
* [Sign in or register](./sign-in-dbt-extension.md) for a dbt platform account to keep using advanced features after the 14-day trial.
* Review the [limitations and unsupported features](./dbt/supported-features.md#limitations).

 Install dbt v2 from the command line, if you haven't already.

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
brew install dbt-labs/dbt/dbt
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

## Getting started

After v2 and the dbt VS Code extension are installed, the dbt logo appears in the sidebar. Click it to open the **Get started** panel.

The **Get started** panel is a persistent setup companion available in VS Code and Cursor. It monitors your environment and shows the most important next action. As new dbt v2 releases ship or your project changes, the panel resurfaces relevant steps automatically. For more details refer to [Upgrade to dbt v2](./upgrade-to-dbt-extension.md).

![The Get started panel in VS Code showing the setup steps, with the active step highlighted.](/img/docs/extension/vsce-get-started.png?v=2 "The Get started panel in VS Code showing the setup steps, with the active step highlighted.")The Get started panel in VS Code showing the setup steps, with the active step highlighted.

The panel guides you through:

1. **Install or update dbt v2**: Detects whether the dbt v2 binary is missing or outdated and installs or updates it with a single click.
2. **Open project**: Checks for a `dbt_project.yml` file in your workspace to confirm a valid dbt project is open before proceeding.
3. **Check dbt v2 compatibility**: Guides you through upgrading your project to dbt v2. You can choose between an agentic migration or manual CLI onboarding — refer to [Upgrade to dbt v2](./upgrade-to-dbt-extension.md).
4. **Register**: Confirms you've registered your email to use the extension beyond the 14-day trial period — refer to [Sign in or register](./sign-in-dbt-extension.md).

When all setup steps are complete, the panel shows a green **Extension setup complete** button.

![The Get started panel showing Extension setup complete with all four steps checked.](/img/docs/extension/vsce-get-started-complete.png?v=2 "The Get started panel showing Extension setup complete with all four steps checked.")The Get started panel showing Extension setup complete with all four steps checked.

## Next steps

Once you've installed the dbt VS Code extension, go to the next pages to get started:

1. Review the [Upgrade to dbt v2](./upgrade-to-dbt-extension.md) page to upgrade your dbt project to the next-gen engine today!
2. [Sign in or register](./sign-in-dbt-extension.md) for a free dbt platform account to keep using advanced features after the 14-day trial.
3. Review the [limitations and unsupported features](./dbt/supported-features.md#limitations).
