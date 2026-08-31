# Install dbt

Local development(Applies to dbt v2.0 and later)

Get dbt running on your machine in a few minutes. Installing dbt gives you Fusion by default: the current, free-to-use experience for v2. Choose your preferred installation method:

## Install dbt [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

### pip

```shell
python -m pip install --pre dbt
```

To upgrade later, run `python -m pip install --upgrade --pre dbt`.

### Homebrew (macOS)

```shell
brew tap dbt-labs/dbt
brew install dbt-labs/dbt/dbt
```

To upgrade later, run `brew upgrade dbt`.

### curl (macOS/Linux)

```shell
curl -fsSL https://public.cdn.getdbt.com/fs/install/install.sh | sh -s -- --update
```

This installs the dbt binary to `~/.local/bin/dbt` and adds that directory to your `$PATH`. See [uninstalling a curl install](#faqs) if you ever need to remove it.

Close and reopen your terminal (or run `exec $SHELL`) so the new `$PATH` is recognized.

To upgrade later, run `dbt system update`.

### winget (Windows)

```shell
winget install --id dbtLabs.dbt --exact
```

To install a specific version, run `winget install --id dbtLabs.dbt --exact --version <version>`.

### Windows (PowerShell)

```powershell
irm https://public.cdn.getdbt.com/fs/install/install.ps1 | iex
```

Close and reopen your shell (or run `Start-Process powershell`) so the new `Path` is recognized.

To upgrade later, run `dbt system update`.

* Verify your installation:

  ```shell
  dbt --version
  ```

* With dbt v2, you can start using the Fusion experience right away. For the best v2 editor experience, install the dbt VS Code extension to use features like autocomplete, inline errors, and lineage.

  For full LSP features and other richer Fusion capabilities, run `dbt login` to sign in with a free dbt platform account:

  ```shell
  dbt login
  ```

Refer to the [dbt VS Code extension docs](../about-dbt-extension.md) for more info.

If you or your org has a strict requirement to use the open-source runtime, install it [here](./install-dbt-v2.md).

## Troubleshooting

Common issues and resolutions:

* **dbt command not found:** Add the installation location to your `$PATH`.
* **Version conflicts:** Check that no other dbt Core or dbt platform CLI versions are installed or active on your machine.
* **Installation permissions:** Make sure your user account can install software locally.

## FAQs

*  Can I revert to my previous dbt installation?

  Yes. To test a new install without affecting your existing workflows, use a separate environment or virtual machine.

*  Can I download the Apache 2.0 runtime only?

  Yes if you need to use the Apache 2.0 runtime, you can [install open source dbt v2](./install-dbt-v2.md), the open-source project behind Fusion.

*  How do I uninstall a curl (install.sh) install?

  These steps apply only if you installed Fusion with the curl (`install.sh`) script. If you used pip, Homebrew, or winget, remove dbt with that tool instead (for example, `pip uninstall dbt` or `brew uninstall dbt`).

  1. **Uninstall dbt.** Run the built-in uninstall command to clear cached files and state. This also removes the binary for you:

     ```shell
     dbt system uninstall
     ```

  2. **Clean up your shell profile.** The installer adds a `$PATH` export and a `dbtf` alias to `~/.zshrc` or `~/.bashrc`, each under its own comment. Open that file and delete these lines:

     ```shell
     # Added by dbt installer
     export PATH="$PATH:$HOME/.local/bin"

     # dbt aliases
     alias dbtf=$HOME/.local/bin/dbt
     ```

     Then reload your profile:

     ```shell
     source ~/.zshrc   # or source ~/.bashrc
     ```

## More information about dbt v2

* [About the dbt extension](../about-dbt-extension.md)
* [Supported features matrix](../dbt/supported-features.md)
* [Install dbt](./install-dbt.md)
* [Quickstart for Fusion](../../guides/dbt.md?step=1)
* [Upgrade guide](../dbt-versions/dbt-upgrade/upgrading-to-v2.md)
* [dbt v2 license agreement](https://www.getdbt.com/dbt-fusion-engine-license-agreement)

## Next steps

* Configure [environment variables](./configure-environment-variables.md) to manage credentials.
* Configure your [profiles.yml](./profiles.yml.md#location-of-profilesyml) file.
* Configure your [data platform connection](./connect-data-platform/about-dbt-connections.md).
* Create your first [dbt project](../build/projects.md) using the [`dbt init`](../../reference/commands/init.md) command.
