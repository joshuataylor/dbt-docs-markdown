# Install dbt

Local developmentⓘ

Get dbt running on your machine in a few minutes. Installing dbt gives you Fusion by default: the current, free-to-use experience for v2. Choose your preferred installation method:

## Install dbt [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")[​](#install-dbt- "Direct link to install-dbt-")

* pip
* Homebrew (macOS)
* curl (macOS/Linux)
* winget (Windows)
* Windows (PowerShell)

```shell
python -m pip install --pre dbt
```

To upgrade later, run `python -m pip install --upgrade --pre dbt`.

```shell
brew install dbt
```

To upgrade later, run `brew upgrade dbt`.

```shell
curl -fsSL https://public.cdn.getdbt.com/fs/install/install.sh | sh -s -- --update
```

Close and reopen your terminal (or run `exec $SHELL`) so the new `$PATH` is recognized.

To upgrade later, run `dbt system update`.

```shell
winget install --id dbtLabs.dbt --exact
```

To install a specific version, run `winget install --id dbtLabs.dbt --exact --version <version>`.

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

Refer to the [dbt VS Code extension docs](https://docs.getdbt.com/docs/about-dbt-extension.md) for more info.

If you or your org has a strict requirement to use the open-source runtime, install it [here](https://docs.getdbt.com/docs/local/install-dbt-core-v2.md).

## Troubleshooting[​](#troubleshooting "Direct link to Troubleshooting")

Common issues and resolutions:

* **dbt command not found:** Add the installation location to your `$PATH`.
* **Version conflicts:** Check that no other dbt Core or dbt CLI versions are installed or active on your machine.
* **Installation permissions:** Make sure your user account can install software locally.

## Frequently asked questions[​](#frequently-asked-questions "Direct link to Frequently asked questions")

*  Can I revert to my previous dbt installation?

  Yes. To test a new install without affecting your existing workflows, use a separate environment or virtual machine.

*  Can I download the Apache 2.0 runtime only?

  Yes if you need to use the Apache 2.0 runtime, you can [install dbt Core 2.0](https://docs.getdbt.com/docs/local/install-dbt-core-v2.md), the open-source project behind Fusion.

## More information about Fusion[​](#more-information-about-fusion "Direct link to More information about Fusion")

* [About the dbt extension](https://docs.getdbt.com/docs/about-dbt-extension.md)
* [Supported features matrix](https://docs.getdbt.com/docs/fusion/supported-features.md)
* [Install dbt](https://docs.getdbt.com/docs/local/install-dbt.md)
* [Quickstart for Fusion](https://docs.getdbt.com/guides/fusion.md?step=1)
* [Upgrade guide](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md)
* [Fusion license agreement](https://www.getdbt.com/dbt-fusion-engine-license-agreement)

<!-- -->

## Next steps[​](#next-steps "Direct link to Next steps")

* Configure [environment variables](https://docs.getdbt.com/docs/local/configure-environment-variables.md) to manage credentials.
* Configure your [profiles.yml](https://docs.getdbt.com/docs/local/profiles.yml.md#location-of-profilesyml) file.
* Configure your [data platform connection](https://docs.getdbt.com/docs/local/connect-data-platform/about-dbt-connections.md).
* Create your first [dbt project](https://docs.getdbt.com/docs/build/projects.md) using the [`dbt init`](https://docs.getdbt.com/reference/commands/init.md) command.
