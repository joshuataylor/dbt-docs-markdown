# Hybrid development with dbt platform and Fusion

[Back to guides](https://docs.getdbt.com/guides.md)

dbt Fusion engine

dbt platform

Local development

Beginner

[Menu ]()



## Introduction[​](#introduction "Direct link to Introduction")

Hybrid dbt deployments are becoming increasingly common. Fusion adopters are frequently working in several places at once: in the dbt platform for production runs and IDE-based development, and locally using the dbt platform CLI or the dbt VS Code extension.

These paths are fully supported for dbt platform users. Keeping the environments in sync across credentials, environment variables, and engine versions is one of the first operational challenges teams encounter.

This guide walks through credentials, environment variables, Fusion versions, and Mesh or deferral, with concrete, copy-paste-ready steps to keep everything aligned.

## Prerequisites[​](#prerequisites "Direct link to Prerequisites")

* You have a dbt platform account with at least one project using the dbt Fusion engine.
* You have either the [dbt platform CLI](https://docs.getdbt.com/docs/platform/dbt-cli-installation.md) or the [dbt VS Code extension + local Fusion CLI](https://docs.getdbt.com/docs/local/install-dbt.md?version=2.0#installation) installed.

## 1. Managing credentials[​](#1-managing-credentials "Direct link to 1. Managing credentials")

How you authenticate to your data warehouse locally depends on which local tool you use:

* [dbt platform CLI](https://docs.getdbt.com/guides/fusion-platform-local-workflow.md?step=3#dbt-platform-cli): For a CLI-only local development experience (without the dbt VS Code extension), use the dbt platform CLI with Fusion set as your platform release track. Warehouse credentials are managed centrally in dbt platform and passed through automatically — no `profiles.yml` required.
* [dbt VS Code extension](https://docs.getdbt.com/guides/fusion-platform-local-workflow.md?step=3#dbt-vs-code-extension-profilesyml-required): For IDE-based local development, the dbt VS Code extension runs the dbt Fusion engine and its LSP features in a local process. This path requires a `profiles.yml` to connect directly to your warehouse.

### dbt platform CLI[​](#dbt-platform-cli "Direct link to dbt platform CLI")

The [dbt CLI](https://docs.getdbt.com/docs/platform/dbt-cli-installation.md) is the lowest-friction path for dbt platform users who want a CLI-only local workflow without VS Code. It authenticates using your dbt platform session, and your warehouse credentials are managed centrally in dbt platform and passed through automatically.

For detailed installation instructions, refer to [Install the dbt CLI](https://docs.getdbt.com/docs/platform/dbt-cli-installation.md?version=1.10).

The dbt platform CLI is installed from your local command prompt. For example:

```shell
# Install the dbt platform CLI (if not already installed)
pip3 install dbt

# Verify your connection
dbt debug
```

The configuration file downloaded from your dbt platform **Account settings** will facilitate the connection and authentication with your existing credentials.

This is the lowest-friction path for teams that don't need full IDE integration locally.

### dbt VS Code extension (profiles.yml required)[​](#dbt-vs-code-extension-profilesyml-required "Direct link to dbt VS Code extension (profiles.yml required)")

The dbt VS Code extension runs Fusion and its language server in a local process and connects directly to your warehouse. For this reason, you need a `profiles.yml` for local extension development sessions.

Download your [`dbt_cloud.yml`](https://docs.getdbt.com/reference/dbt_cloud.yml.md) from your dbt platform **Account settings** and Fusion attempts to hydrate non-sensitive credential metadata from dbt platform automatically. To avoid manually recreating your warehouse configuration, use `dbt init`.

```shell
dbt init
```

Fusion pulls down fields such as your **username**, **role**, **warehouse**, **database**, and **schema**, but never sensitive values like passwords or tokens. If your authentication mechanism is passwordless (such as `externalbrowser` or SSO-based OAuth), Fusion configures that too, so you can work without storing secrets locally.

note

This hydration happens once during initial setup and does not stay in sync automatically. When your warehouse configuration changes in dbt platform, run `dbt init` again to refresh your local `profiles.yml`.

The dbt VS Code extension first-time setup flow prompts you through this process, so you usually don't need to run `dbt init` manually.

Coming soon

We're working on a solution that lets you develop locally in the dbt VS Code extension while you manage credentials entirely in dbt platform, without a local `profiles.yml`. We'll update this page when that ships.

## 2. Managing environment variables[​](#2-managing-environment-variables "Direct link to 2. Managing environment variables")

Environment variables you set in dbt platform apply to production runs and the Studio IDE sessions. For local development, you manage environment variables separately.

### dbt platform CLI[​](#dbt-platform-cli-1 "Direct link to dbt platform CLI")

When you use the dbt platform CLI, dbt platform injects the same environment variables you use in production into your dbt CLI session. You don't need extra setup.

### VS Code extension (.env file)[​](#vs-code-extension-env-file "Direct link to VS Code extension (.env file)")

The dbt VS Code extension runs Fusion as a local process, so environment variables from dbt platform are not automatically available. Instead, use a [`.env` file](https://dotenvx.com/docs) at the root of your dbt project:

```shell
# .env
DBT_MY_DATABASE=my_database
DBT_MY_SCHEMA=my_dev_schema
DBT_TARGET_SCHEMA=analytics_dev
```

Fusion and the dbt VS Code extension automatically load values from this file. You can also view and override individual environment variables from the extension's settings UI.

Reference these variables in your `profiles.yml` or elsewhere in your dbt project using the [`env_var` Jinja function](https://docs.getdbt.com/reference/dbt-jinja-functions/env_var.md):

```yaml
# profiles.yml
my_profile:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: my_account
      database: "{{ env_var('DBT_MY_DATABASE') }}"
      schema: "{{ env_var('DBT_MY_SCHEMA') }}"
```

For a full walkthrough of `.env` file usage and variable precedence, see [Environment variables](https://docs.getdbt.com/docs/build/environment-variables.md) for more information on where to find your configured variables and [Set environment variables locally](https://docs.getdbt.com/docs/configure-dbt-extension.md?version=2.0#set-environment-variables-locally) for local configuration instructions.

### Keeping platform and local variables in sync[​](#keeping-platform-and-local-variables-in-sync "Direct link to Keeping platform and local variables in sync")

Environment variables in dbt platform and a local `.env` file don't sync automatically.

Commit a `.env.example` file with the list of required variables to your repo. Each developer copies it locally and fills in their own values and their copy is never committed.

```shell
# .env.example (committed to version control)
DBT_MY_DATABASE=            # Your development database name
DBT_MY_SCHEMA=              # Your personal dev schema, for example dbt_yourname
DBT_TARGET_SCHEMA=          # Target schema for dbt output
```

```shell
# Developer setup: copy the example and fill in your values
cp .env.example .env
```

Do not commit .env

Fusion and the dbt VS Code extension only load from a file named exactly `.env`, so each developer needs their own copy. Make sure `.env` is in your `.gitignore` so credentials are never committed. Running `dbt init` adds this automatically.

```shell
echo ".env" >> .gitignore
```

When environment variables change in dbt platform (you add variables or rename values), update `.env.example` in the same pull request so local developers know to update their own `.env`.

For teams with strict security requirements

Consider a script that fetches variables from your secrets manager (for example, AWS Secrets Manager or 1Password) and writes them to `.env` at the start of a session, instead of storing values in a file long term.

## 3. Managing Fusion versions[​](#3-managing-fusion-versions "Direct link to 3. Managing Fusion versions")

The **Latest** release track on dbt platform updates continuously as Fusion ships new releases. If your local version falls behind, you might see inconsistent behavior. The same query could compile differently locally than in production, or a feature might exist in dbt platform but not in your local binary. Stay current to avoid these mismatches.

### Versions on the dbt platform[​](#versions-on-the-dbt-platform "Direct link to Versions on the dbt platform")

On dbt platform, Fusion follows a versionless release track model. The default release track is **Fusion Stable**, which always runs the most recent stable release. For details on release tracks and their stability levels, see [Fusion releases](https://docs.getdbt.com/docs/dbt-versions/dbt-release-tracks.md#fusion-release-tracks).

### Versions installed locally[​](#versions-installed-locally "Direct link to Versions installed locally")

By default, the Fusion [installation script](https://docs.getdbt.com/docs/local/install-dbt.md) installs the latest stable release, the same version that ships with the **Fusion Stable** release track on dbt platform:

```shell
# macOS / Linux
curl -fsSL https://downloads.getdbt.com/install/dbt-fusion.sh | sh
```

To update your local installation to the latest stable release at any time:

```shell
dbt system update
```

To check your current version:

```shell
dbt --version
```

### Keeping versions in sync: dev containers (recommended)[​](#keeping-versions-in-sync-dev-containers-recommended "Direct link to Keeping versions in sync: dev containers (recommended)")

Use a [VS Code dev container](https://code.visualstudio.com/docs/devcontainers/containers) for the most reliable match between local Fusion versions and dbt platform. A dev container runs your environment inside a Docker image that rebuilds at the start of each session and performs a fresh Fusion install each time, so everyone on your team uses the same version as dbt platform without manual updates.

Our friends at Brooklyn Data have published a ready-to-use Fusion dev container:

* **Dev container template:** [brooklyn-data/dbt-fusion-devcontainer](https://github.com/brooklyn-data/dbt-fusion-devcontainer)
* **Blog post:** [Why you should use dev containers with dbt Fusion](https://www.brooklyndata.co/ideas/2025/06/11/why-you-should-use-dev-containers-with-dbt-fusion)

To get started with their template:

```shell
# Clone the devcontainer template into your project
curl -fsSL https://raw.githubusercontent.com/brooklyn-data/dbt-fusion-devcontainer/main/setup.sh | sh
```

Then open your project in VS Code and select **Reopen in Container** when prompted. VS Code builds the image and installs the latest stable Fusion release automatically.

Coming soon

We're introducing additional Fusion release tracks on dbt platform beyond **Fusion Stable**. When they're available, we'll update this guide with steps to pin your dev container to a specific track.

### Without dev containers: update at the start of each session[​](#without-dev-containers-update-at-the-start-of-each-session "Direct link to Without dev containers: update at the start of each session")

If dev containers aren't an option for your team, run `dbt system update` at the start of each development session instead. That installs the latest stable release, the same version as the **Latest** track on dbt platform, so your local binary stays current:

```shell
dbt system update && dbt debug
```

Pinning to a specific version number does not work long term here: the **Latest** track on dbt platform keeps advancing, and a pinned local install falls behind. Aim to stay on **Latest** instead of locking to one release.

To make this easy to remember, add a `dev` target to your project's `Makefile`:

```makefile
# Makefile
.PHONY: dev
dev:
	dbt system update
	dbt debug
```

Then developers start their session with:

```shell
make dev
```

You can also document this convention in your project's `CONTRIBUTING.md` so it's part of your onboarding checklist.

***

## 4. dbt Mesh and deferral[​](#4-dbt-mesh-and-deferral "Direct link to 4. dbt Mesh and deferral")

If your project uses [dbt Mesh](https://docs.getdbt.com/docs/mesh/about-mesh.md), referencing models from other dbt projects via cross-project refs, Fusion handles this automatically during local development when a [`dbt_cloud.yml`](https://docs.getdbt.com/reference/dbt_cloud.yml.md) is present.

### How it works[​](#how-it-works "Direct link to How it works")

When Fusion detects upstream projects defined in your `dependencies.yml`, it downloads the publication artifact for each upstream project from dbt platform before resolving cross-project refs. Then `ref('upstream_project', 'model_name')` works locally without manual setup.

Your logs include lines such as the following while Fusion resolves cross-project refs:

```text
Downloading publication artifact for <upstream_project> (resolving cross-project refs)
Downloaded publication artifact for <upstream_project> to <path> (resolving cross-project refs)
```

Fusion caches downloaded publication artifacts for up to one hour, so subsequent runs in the same session skip the download and resolve refs from the local cache.

Auto-deferral is also on by default. When a [`dbt_cloud.yml`](https://docs.getdbt.com/reference/dbt_cloud.yml.md) is present, Fusion defers to your project's configured deferral environment, so you build only modified models and their downstream dependencies while the rest resolve against the production state.

### Disabling deferral[​](#disabling-deferral "Direct link to Disabling deferral")

* **In the VS Code extension:** Disable auto-deferral in the extension settings. Search for `Dbt > Flag: Defer` and uncheck the option:

  [![dbt VS Code extension deferral settings](/img/fusion/vsce-defer-settings.png?v=2 "dbt VS Code extension deferral settings")](#)dbt VS Code extension deferral settings

* **On the CLI:** Pass `--no-defer` to any command to skip both deferral and the publication artifact download:

  ```shell
  dbt run --no-defer
  dbt compile --no-defer
  ```

## Reference table[​](#reference-table "Direct link to Reference table")

The following table summarizes the key differences between the two local development paths covered in this guide:

| Area                      | dbt platform CLI                                                                                                         | dbt VS Code extension                                                                                                             |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------- |
| **Credentials**           | Managed through your dbt platform session, no `profiles.yml` needed                                                      | `profiles.yml` required; use `dbt init` to hydrate from dbt platform                                                              |
| **Environment variables** | Same env vars as in dbt platform automatically                                                                           | Use a `.env` file at the project root                                                                                             |
| **Version management**    | `dbt system update` to stay current                                                                                      | Dev container recommended for automatic sync                                                                                      |
| **dbt Mesh / deferral**   | Auto-enabled when [`dbt_cloud.yml`](https://docs.getdbt.com/reference/dbt_cloud.yml.md) present; `--no-defer` to disable | Auto-enabled when [`dbt_cloud.yml`](https://docs.getdbt.com/reference/dbt_cloud.yml.md) present; toggle off in extension settings |

## Related docs[​](#related-docs "Direct link to Related docs")

* [Install Fusion](https://docs.getdbt.com/docs/local/install-dbt.md)
* [dbt platform CLI installation](https://docs.getdbt.com/docs/platform/dbt-cli-installation.md)
* [Fusion releases and release channels](https://docs.getdbt.com/docs/fusion/fusion-releases.md)
* [About profiles.yml](https://docs.getdbt.com/docs/local/profiles.yml.md)
* [Environment variables (local)](https://docs.getdbt.com/docs/local/configure-environment-variables.md)
* [VS Code dev containers](https://code.visualstudio.com/docs/devcontainers/containers)
* [dbt Mesh overview](https://docs.getdbt.com/docs/mesh/about-mesh.md)
* [Deferral in dbt](https://docs.getdbt.com/docs/platform/about-defer.md)
