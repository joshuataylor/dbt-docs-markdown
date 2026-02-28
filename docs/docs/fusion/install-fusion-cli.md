# Install Fusion from the CLI [Preview](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Fusion can be installed via the command line from our official content delivery network (CDN). Fusion CLI delivers dbt Fusion engine performance benefits (faster parsing, compilation, execution) but does not include LSP features. For the best dbt Fusion engine experience, [install the dbt VS Code extension](https://docs.getdbt.com/docs/install-dbt-extension.md) in your VS Code or compatible IDE.

If you already have the dbt Fusion engine installed, you can skip this step. If you don't have it installed, you can follow these steps to install it:

1. Open a new command-line window and run the following command to install the dbt Fusion engine:

   * macOS & Linux
   * Windows (PowerShell)

   Run the following command in the terminal:

   ```shell
   curl -fsSL https://public.cdn.getdbt.com/fs/install/install.sh | sh -s -- --update
   ```

   To use `dbtf` immediately after installation, reload your shell so that the new `$PATH` is recognized:

   ```shell
   exec $SHELL
   ```

   Or, close and reopen your Terminal window. This will load the updated environment settings into the new session.

   Run the following command in PowerShell:

   ```powershell
   irm https://public.cdn.getdbt.com/fs/install/install.ps1 | iex
   ```

   To use `dbtf` immediately after installation, reload your shell so that the new `Path` is recognized:

   ```powershell
   Start-Process powershell
   ```

   Or, close and reopen PowerShell. This will load the updated environment settings into the new session.

2. Run the following command to verify you've installed Fusion:

   ```bash
   dbtf --version
   ```

   You can use `dbt` or its Fusion alias `dbtf` (handy if you already have another dbt CLI installed). Default install path:

   * macOS/Linux: `$HOME/.local/bin/dbt`
   * Windows: `C:\Users\<username>\.local\bin\dbt.exe`

   The installer adds this path automatically, but you may need to reload your shell for the `dbtf` command to work.

## Update Fusion[​](#update-fusion "Direct link to Update Fusion")

The following command will update to the latest version of Fusion and adapter code:

```shell
dbtf system update
```

## Uninstall Fusion[​](#uninstall-fusion "Direct link to Uninstall Fusion")

This command will uninstall the Fusion binary from your system, but aliases will remain wherever they are installed (for example `~/.zshrc`):

```shell
dbtf system uninstall
```

## Adapter installation[​](#adapter-installation "Direct link to Adapter installation")

The Fusion install automatically includes adapters outlined in the [Fusion requirements](https://docs.getdbt.com/docs/fusion/supported-features.md#requirements). Other adapters will be available at a later date.

## Environment variables[​](#environment-variables "Direct link to Environment variables")

Fusion automatically loads environment variables from a `.env` file in your current working directory (the folder you `cd` into and run dbt commands from in your terminal). This helps you manage credentials and settings without hardcoding them in your `profiles.yml` or exposing them in your shell history.

### Using a `.env` file[​](#using-a-env-file "Direct link to using-a-env-file")

1. Create a `.env` file in your current working directory (typically at the root of your dbt project):

   ```env
   DBT_MY_DATABASE=my_database
   DBT_MY_SCHEMA=my_schema
   DBT_SECRET_KEY=my_secret_value
   ```

2. Reference these variables in your `profiles.yml` using the [`env_var` Jinja function](https://docs.getdbt.com/reference/dbt-jinja-functions/env_var.md):

   ```yaml
   my_profile:
     target: dev
     outputs:
       dev:
         type: snowflake
         account: my_account
         database: "{{ env_var('DBT_MY_DATABASE') }}"
         schema: "{{ env_var('DBT_MY_SCHEMA') }}"
   ```

3. Run dbt commands normally. Fusion will automatically load the variables from the `.env` file. For example, running `dbtf debug` will show your connection using the values from `.env`:

   ```shell
   dbtf debug
   ...
   Debugging connection:
   "authenticator": "my_authenticator",
   "account": "my_account",
   "user": "my_user",
   "database": "my_database",        # Loaded from DBT_MY_DATABASE in .env
   "schema": "my_schema",            # Loaded from DBT_MY_SCHEMA in .env
   ```

note

We recommend placing your `.env` file in the project root and running dbt commands from that location because the file is loaded *only* from your current working directory. It doesn't support the `--project-dir` flag or `DBT_PROJECT_DIR` environment variable, and dbt won't search your project root if you're running commands from a different directory location.

### Precedence order[​](#precedence-order "Direct link to Precedence order")

When the same environment variable is defined in multiple places, Fusion uses the following precedence order (highest to lowest):

1. Shell environment — Variables set directly in your shell (for example, `export DBT_MY_VAR=value`)
2. `.env` file — Variables defined in the `.env` file in your current working directory

This means environment variables set in your shell always override values from the `.env` file.

tip

Add `.env` to your `.gitignore` file to prevent sensitive credentials from being committed to version control. The `dbtf init` command automatically includes `.env` in the generated `.gitignore` file.

For more details on managing environment variables locally, refer to [Configure your local environment](https://docs.getdbt.com/docs/configure-dbt-extension.md#set-environment-variables-locally).

## profiles.yml location[​](#profilesyml-location "Direct link to profiles.yml location")

Fusion searches for `profiles.yml` in the `--profiles-dir` flag (if specified), project root directory, or `~/.dbt/` directory. Unlike dbt Core, Fusion does not support the `DBT_PROFILES_DIR` environment variable or `profiles.yml` in arbitrary working directories.

For complete details on profiles.yml configuration and search order, refer to [About profiles.yml](https://docs.getdbt.com/docs/fusion/connect-data-platform-fusion/profiles.yml.md#location-of-profilesyml).

## Troubleshooting[​](#troubleshooting "Direct link to Troubleshooting")

Common issues and resolutions:

* **dbt command not found:** Ensure installation location is correctly added to your `$PATH`.
* **Version conflicts:** Verify no existing dbt Core or dbt CLI versions are installed (or active) that could conflict with Fusion.
* **Installation permissions:** Confirm your user has appropriate permissions to install software locally.

## Frequently asked questions[​](#frequently-asked-questions "Direct link to Frequently asked questions")

* Can I revert to my previous dbt installation?

  Yes. If you want to test Fusion without affecting your existing workflows, consider isolating or managing your installation via separate environments or virtual machines.

<!-- -->

## More information about Fusion[​](#more-information-about-fusion "Direct link to More information about Fusion")

Fusion marks a significant update to dbt. While many of the workflows you've grown accustomed to remain unchanged, there are a lot of new ideas, and a lot of old ones going away. The following is a list of the full scope of our current release of the Fusion engine, including implementation, installation, deprecations, and limitations:

* [About the dbt Fusion engine](https://docs.getdbt.com/docs/fusion/about-fusion.md)
* [About the dbt extension](https://docs.getdbt.com/docs/about-dbt-extension.md)
* [New concepts in Fusion](https://docs.getdbt.com/docs/fusion/new-concepts.md)
* [Supported features matrix](https://docs.getdbt.com/docs/fusion/supported-features.md)
* [Installing Fusion CLI](https://docs.getdbt.com/docs/fusion/install-fusion-cli.md)
* [Installing VS Code extension](https://docs.getdbt.com/docs/install-dbt-extension.md)
* [Fusion release track](https://docs.getdbt.com/docs/dbt-versions/upgrade-dbt-version-in-cloud.md#dbt-fusion-engine)
* [Quickstart for Fusion](https://docs.getdbt.com/guides/fusion.md?step=1)
* [Upgrade guide](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-fusion.md)
* [Fusion licensing](http://www.getdbt.com/licenses-faq)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
