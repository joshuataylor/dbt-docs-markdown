# Hybrid development with dbt platform and dbt v2

[Back to guides](https://docs.getdbt.com/guides)



## Introduction

Hybrid dbt deployments are becoming increasingly common. dbt v2 adopters are frequently working in several places at once: in the dbt platform for production runs and IDE-based development, and on their local machine using the dbt platform CLI or the dbt VS Code extension.

These paths are fully supported for dbt platform users. Keeping the environments in sync across credentials, environment variables, and engine versions is one of the first operational challenges teams encounter.

This guide walks through command routing, credentials, environment variables, dbt v2 versions, and Mesh or deferral, with concrete, copy-paste-ready steps to keep everything aligned.

If you run both the dbt platform CLI and a local dbt v2 build from the same project, start with [Choosing which dbt runs](./dbt-platform-local-workflow.md?step=3#choosing-which-dbt-runs). Both tools are invoked as `dbt`, and the rest of this guide assumes you can tell them apart.

## Prerequisites

* You have a dbt platform account with at least one project using dbt v2.
* You have either the [dbt platform CLI](../docs/platform/dbt-cli-installation.md) or the [dbt VS Code extension + local dbt](../docs/local/install-dbt.md) installed.

## Choosing which dbt runs

If you install both the dbt platform CLI and a local dbt v2 build, you have two separate programs on your machine that are both invoked by typing `dbt`. Before you configure credentials, environment variables, or versions, make it unambiguous which one you're calling.

Skip this section if you only ever install one of the two.

### The two execution paths

Both the dbt platform CLI and the local dbt v2 execution paths read the same project files — one clone of your repository, one `dbt_project.yml`, one set of models, macros, and tests. What differs is where the work happens and where the connection details come from.

| Area               | dbt platform CLI                                 | Local dbt v2                                    |
| ------------------ | ------------------------------------------------ | ----------------------------------------------- |
| **What it is**     | A client that sends your command to dbt platform | A dbt executable that runs on your machine      |
| **Where dbt runs** | On dbt platform infrastructure                   | Locally, or inside your agent's virtual machine |

### Give each tool its own command

Because both programs are installed as `dbt`, whichever one appears first in your `$PATH` opens when you use `dbt`. To avoid relying on `$PATH` order, you can assign at least one of them an alias that you can use on the command line to call each one explicitly.

The dbt v2 [installation script](../docs/local/install-dbt.md) already provides `dbtf` as an alias and points to the local dbt v2 binary, or executable program. You can also add a `dbt-cli` alias for the dbt platform CLI so each command calls exactly what it says, for example:

```shell
dbtf build --select my_model      # Runs locally, on your installed v2 binary
dbt-cli build --select my_model   # Runs on dbt platform, on your environment's release track
```

Report incorrect code

Follow these steps to set up an alias:

1. Find out what `dbt` resolves to today, and whether more than one is installed:

   ```shell
   which -a dbt
   ```

   Report incorrect code

2. Install the tools you need:

   * Install dbt v2 using the [installation script](../docs/local/install-dbt.md), which puts it in `$HOME/.local/bin/dbt` on macOS and Linux, or `C:\Users\USERNAME\.local\bin\dbt.exe` on Windows, and adds the `dbtf` alias.
   * Install the [dbt platform CLI](../docs/platform/dbt-cli-installation.md) separately.

3. Using the path you identified in Step 1 for your dbt platform CLI install, add an alias to your shell profile that points to its installed program.

   For example, on macOS with Homebrew you can create an alias for the executable at `/opt/homebrew/bin/dbt`:

   ```shell
   # ~/.zshrc or ~/.bashrc
   alias dbt-cli="/opt/homebrew/bin/dbt"
   ```

   Report incorrect code

   Reload your shell profile:

   ```shell
   source ~/.zshrc   # or source ~/.bashrc
   ```

   Report incorrect code

4. Confirm each command resolves to the tool you expect and the two version strings differ:

   ```shell
   dbtf --version
   dbt-cli --version
   ```

   Report incorrect code

5. Decide which program opens when you run bare `dbt` on your machine and create a best practice for your team. Regardless, `dbtf` and `dbt-cli` clearly run the named program.

Shell aliases don't apply everywhere

`dbtf` and `dbt-cli` are shell aliases. They're available in your interactive terminal, but not in `Makefile` recipes, shell scripts, CI jobs, or commands an AI agent runs in a non-interactive shell. In those contexts, use the absolute path to the binary, or control `$PATH` ordering so that bare `dbt` resolves to the tool you want.

### Commands that mean different things in each tool

Most commands (`build`, `run`, `test`, `compile`) behave equivalently, but the run happens in a different place. A few are specific to one tool, and running them against the other either fails or does something you didn't intend:

* `dbt system update` and `dbt system uninstall` manage a local dbt v2 install. They have no meaning for the dbt platform CLI and no effect on dbt platform.
* `dbt init` hydrates a local `profiles.yml`. You need it for the local dbt v2 path, not for the dbt platform CLI, which doesn't use `profiles.yml`.
* `dbt debug` inspects a local profile, target, and connection. Use [`dbt environment`](../reference/commands/dbt-environment.md) for dbt platform CLI environment and connection details.

When both tools are installed, write these as `dbtf system update`, `dbtf init`, and `dbtf debug` so they can't be misread.

### How each editor and agent picks a dbt

Each tool in your workflow resolves `dbt` on its own terms. Configuring one does not configure the others.

| Where you run dbt                                           | How it picks a dbt                                                                     | What to configure                                                                                                                                                           |
| ----------------------------------------------------------- | -------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **dbt VS Code extension**                                   | The `dbt.fusionPath` setting, or a v2 build the extension downloads and manages itself | Leave `dbt.fusionPath` unset to let the extension manage v2. If you install v2 yourself, set it to the absolute path of the v2 binary                                       |
| **VS Code or Cursor integrated terminal**                   | Your shell `$PATH` and shell profile aliases                                           | The `dbtf` and `dbt-cli` aliases described earlier                                                                                                                          |
| **Coding agents, such as Claude Code or Cursor agent mode** | A non-interactive shell — `$PATH` applies, shell aliases usually don't                 | Absolute binary paths, plus a written command-routing rule in the agent's instructions file                                                                                 |
| **Remote agent virtual machines**                           | The virtual machine's own `$PATH`, not your workstation's                              | Install both tools in the virtual machine, persist them in its setup configuration so they survive fresh sessions, and store credentials in that platform's secrets manager |

`dbt.fusionPath` is not a terminal setting

`dbt.fusionPath` tells the dbt VS Code extension which binary to start the LSP and the extension's own menu actions from. It has no effect on commands you type in an integrated terminal, and no effect on commands an agent runs. It must point to a valid dbt v2 binary — an absolute filesystem path, not an alias name and not the dbt platform CLI.

Find the path to pass it with:

```shell
command -v dbtf
```

Report incorrect code

Keep machine-specific absolute paths in your user settings rather than committing them to workspace settings, so the setting doesn't break for teammates whose paths differ.

### Tell your coding agent which command to use

Agents run shell commands the same way a script does, so they inherit `$PATH` but not your interactive aliases, and they have no way to guess which execution path you intended. State the convention in the instructions file the agent reads — `CLAUDE.md`, `AGENTS.md`, `.cursor/rules/`, or the equivalent for your tool:

```markdown
## dbt command routing

Replace the paths below with the output of `which -a dbt` on this machine.

- Use `$HOME/.local/bin/dbt` for local dbt v2 commands. It runs on this machine
  against `profiles.yml`.
- Use `/opt/homebrew/bin/dbt` for dbt platform CLI commands. It runs on
  dbt platform against the environment's release track.
- Never substitute one for the other.
- Run dbt commands from the repository root.
- Ask before running any command that creates or modifies warehouse objects.
```

Report incorrect code

Use absolute paths in agent instructions rather than the `dbtf` and `dbt-cli` aliases, because the agent's shell may not load your shell profile. Discover the real paths on the machine the agent runs on with `which -a dbt`, and update the instructions file when they change. On a remote agent virtual machine, confirm the paths inside a fresh session, because tools installed ad hoc in an earlier session may not persist.

## Managing credentials

How you authenticate to your data warehouse locally depends on which self-hosted tool you use:

* [dbt platform CLI](./dbt-platform-local-workflow.md?step=4#dbt-platform-cli): For a CLI-only development experience (without the dbt VS Code extension), use the dbt platform CLI with dbt v2 set as your platform release track. Warehouse credentials are managed centrally in dbt platform and passed through automatically — no `profiles.yml` required.
* [dbt VS Code extension](./dbt-platform-local-workflow.md?step=4#dbt-vs-code-extension-profilesyml-required): For IDE-based local development, the dbt VS Code extension runs dbt v2 and its LSP features in a local process. This path requires a `profiles.yml` to connect directly to your warehouse.

### dbt platform CLI

The [dbt platform CLI](../docs/platform/dbt-cli-installation.md) is the lowest-friction path for dbt platform users who want a self-hosted CLI-only workflow without VS Code. It authenticates using your dbt platform session, and your warehouse credentials are managed centrally in dbt platform and passed through automatically.

For detailed installation instructions, refer to [Install the dbt platform CLI](../docs/platform/dbt-cli-installation.md?version=1.10). The dbt platform CLI is installed from your local command prompt.

The configuration file downloaded from your dbt platform **Account settings** will facilitate the connection and authentication with your existing credentials.

This is the lowest-friction path for teams that don't need full IDE integration locally.

### dbt VS Code extension (profiles.yml required)

The dbt VS Code extension runs dbt v2 and its language server in a local process and connects directly to your warehouse. For this reason, you need a `profiles.yml` for local extension development sessions.

Download your [`dbt_cloud.yml`](../reference/dbt_cloud.yml.md) from your dbt platform **Account settings** and dbt v2 attempts to hydrate non-sensitive credential metadata from dbt platform automatically.

If you get access to a new project, re-download the `dbt_cloud.yml` file before working on it locally. To switch between projects already listed in your file, update [`context.active-project`](../reference/dbt_cloud.yml.md#update-or-switch-projects). To avoid manually recreating your warehouse configuration, use `dbt init`.

```shell
dbt init
```

Report incorrect code

dbt v2 pulls down fields such as your **username**, **role**, **warehouse**, **database**, and **schema**, but never sensitive values like passwords or tokens. If your authentication mechanism is passwordless (such as `externalbrowser` or SSO-based OAuth), dbt v2 configures that too, so you can work without storing secrets locally.

note

This hydration happens once during initial setup and does not stay in sync automatically. When your warehouse configuration changes in dbt platform, run `dbt init` again to refresh your local `profiles.yml`.

The dbt VS Code extension first-time setup flow prompts you through this process, so you usually don't need to run `dbt init` manually.

Coming soon

We're working on a solution that lets you develop locally in the dbt VS Code extension while you manage credentials entirely in dbt platform, without a local `profiles.yml`. We'll update this page when that ships.

## Managing environment variables

Environment variables you set in dbt platform apply to production runs and the Studio IDE sessions. For local development, you manage environment variables separately.

### dbt platform CLI

When you use the dbt platform CLI, dbt platform injects the same environment variables you use in production into your dbt platform CLI session. You don't need extra setup.

### VS Code extension (.env file)

The dbt VS Code extension runs dbt v2 as a local process, so environment variables from dbt platform are not automatically available. Instead, use a [`.env` file](https://dotenvx.com/docs) at the root of your dbt project:

```shell
# .env
DBT_MY_DATABASE=my_database
DBT_MY_SCHEMA=my_dev_schema
DBT_TARGET_SCHEMA=analytics_dev
```

Report incorrect code

dbt v2 and the dbt VS Code extension automatically load values from this file. You can also view and override individual environment variables from the extension's settings UI.

Reference these variables in your `profiles.yml` or elsewhere in your dbt project using the [`env_var` Jinja function](../reference/dbt-jinja-functions/env_var.md):

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

Report incorrect code

For a full walkthrough of `.env` file usage and variable precedence, see [Environment variables](../docs/build/environment-variables.md) for more information on where to find your configured variables and [Set environment variables locally](../docs/configure-dbt-extension.md?version=2.0#set-environment-variables-locally) for local configuration instructions.

### Keeping platform and local variables in sync

Environment variables in dbt platform and a local `.env` file don't sync automatically.

Commit a `.env.example` file with the list of required variables to your repo. Each developer copies it locally and fills in their own values and their copy is never committed.

```shell
# .env.example (committed to version control)
DBT_MY_DATABASE=            # Your development database name
DBT_MY_SCHEMA=              # Your personal dev schema, for example dbt_yourname
DBT_TARGET_SCHEMA=          # Target schema for dbt output
```

Report incorrect code

```shell
# Developer setup: copy the example and fill in your values
cp .env.example .env
```

Report incorrect code

Do not commit .env

dbt v2 and the dbt VS Code extension only load from a file named exactly `.env`, so each developer needs their own copy. Make sure `.env` is in your `.gitignore` so credentials are never committed. Running `dbt init` adds this automatically.

```shell
echo ".env" >> .gitignore
```

Report incorrect code

When environment variables change in dbt platform (you add variables or rename values), update `.env.example` in the same pull request so local developers know to update their own `.env`.

For teams with strict security requirements

Consider a script that fetches variables from your secrets manager (for example, AWS Secrets Manager or 1Password) and writes them to `.env` at the start of a session, instead of storing values in a file long term.

## Managing dbt v2 versions

The **v2 Stable** release track on dbt platform updates continuously as dbt v2 ships new releases. If your local version falls behind, you might see inconsistent behavior. The same query could compile differently locally than in production, or a feature might exist in dbt platform but not in your local binary. Stay current to avoid these mismatches.

A v2 release track does not change your local dbt

Moving a dbt platform environment to a v2 release track changes the engine that dbt platform uses for that environment. It does not install, update, replace, or select the dbt v2 executable on your machine, and it does not turn the dbt platform CLI into dbt v2.

The reverse is also true: running `dbt system update` locally updates your local install only. It has no effect on which build your dbt platform environments run.

Treat the two version settings as independent, and keep them aligned yourself using the steps in this section.

### Versions on the dbt platform

On dbt platform, dbt v2 follows a versionless release track model. The default release track is **v2 Stable**, which always runs the most recent stable release. For details on release tracks and their stability levels, see [dbt v2 releases](../docs/dbt-versions/dbt-release-tracks.md#dbt-v2-release-tracks).

### Versions installed locally

By default, the dbt v2 [installation script](../docs/local/install-dbt.md) installs the latest stable release, the same version that ships with the **v2 Stable** release track on dbt platform:

```shell
# macOS / Linux
curl -fsSL https://downloads.getdbt.com/install/dbt-fusion.sh | sh
```

Report incorrect code

To update your self-hosted installation to the latest stable release at any time:

```shell
dbt system update
```

Report incorrect code

To check your current version:

```shell
dbt --version
```

Report incorrect code

### Keeping versions in sync: dev containers (recommended)

Use a [VS Code dev container](https://code.visualstudio.com/docs/devcontainers/containers) for the most reliable match between local dbt v2 versions and dbt platform. A dev container runs your environment inside a Docker image that rebuilds at the start of each session and performs a fresh dbt v2 install each time, so everyone on your team uses the same version as dbt platform without manual updates.

Our friends at Brooklyn Data have published a ready-to-use dbt v2 dev container:

* **Dev container template:** [brooklyn-data/dbt-fusion-devcontainer](https://github.com/brooklyn-data/dbt-fusion-devcontainer)
* **Blog post:** [Why you should use dev containers with dbt v2](https://www.brooklyndata.co/ideas/2025/06/11/why-you-should-use-dev-containers-with-dbt-fusion)

To get started with their template:

```shell
# Clone the devcontainer template into your project
curl -fsSL https://raw.githubusercontent.com/brooklyn-data/dbt-fusion-devcontainer/main/setup.sh | sh
```

Report incorrect code

Then open your project in VS Code and select **Reopen in Container** when prompted. VS Code builds the image and installs the latest stable dbt v2 release automatically.

Coming soon

We're introducing additional dbt v2 release tracks on dbt platform beyond **v2 Stable**. When they're available, we'll update this guide with steps to pin your dev container to a specific track.

### Without dev containers: update at the start of each session

If dev containers aren't an option for your team, run `dbt system update` at the start of each development session instead. That installs the latest stable release, the same version as the **v2 Stable** track on dbt platform, so your local binary stays current:

```shell
dbt system update && dbt debug
```

Report incorrect code

Pinning to a specific version number does not work long term here: the **v2 Stable** track on dbt platform keeps advancing, and a pinned self-hosted installation falls behind. Aim to stay on **v2 Stable** instead of locking to one release.

To make this easy to remember, add a `dev` target to your project's `Makefile`:

```makefile
# Makefile
.PHONY: dev
dev:
	dbt system update
	dbt debug
```

Report incorrect code

Then developers start their session with:

```shell
make dev
```

Report incorrect code

You can also document this convention in your project's `CONTRIBUTING.md` so it's part of your onboarding checklist.

***

## dbt Mesh and deferral

If your project uses [dbt Mesh](../docs/mesh/about-mesh.md), referencing models from other dbt projects via cross-project refs, dbt v2 handles this automatically during development when a [`dbt_cloud.yml`](../reference/dbt_cloud.yml.md) is present.

### How it works

When dbt v2 detects upstream projects defined in your `dependencies.yml`, it downloads the publication artifact for each upstream project from dbt platform before resolving cross-project refs. Then `ref('upstream_project', 'model_name')` works locally without manual setup.

Your logs include lines such as the following while dbt v2 resolves cross-project refs:

```text
Downloading publication artifact for <upstream_project> (resolving cross-project refs)
Downloaded publication artifact for <upstream_project> to <path> (resolving cross-project refs)
```

Report incorrect code

dbt v2 caches downloaded publication artifacts for up to one hour, so subsequent runs in the same session skip the download and resolve refs from the local cache.

Auto-deferral is also on by default. When a [`dbt_cloud.yml`](../reference/dbt_cloud.yml.md) is present, dbt v2 defers to your project's configured deferral environment, so you build only modified models and their downstream dependencies while the rest resolve against the production state.

### Disabling deferral

* **In the VS Code extension:** Disable auto-deferral in the extension settings. Search for `Dbt > Flag: Defer` and uncheck the option:

  ![dbt VS Code extension deferral settings](/img/v2/vsce-defer-settings.png?v=2 "dbt VS Code extension deferral settings")dbt VS Code extension deferral settings

* **On the CLI:** Pass `--no-defer` to any command to skip both deferral and the publication artifact download:

  ```shell
  dbt run --no-defer
  dbt compile --no-defer
  ```

  Report incorrect code

## Reference table

The following table summarizes the key differences between the two development paths covered in this guide:

| Area                                | dbt platform CLI                                                                                                         | dbt VS Code extension                                                                                                             |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------- |
| **Command when both are installed** | `dbt-cli` (alias you add)                                                                                                | `dbtf` (alias the installer adds)                                                                                                 |
| **Credentials**                     | Managed through your dbt platform session, no `profiles.yml` needed                                                      | `profiles.yml` required; use `dbt init` to hydrate from dbt platform                                                              |
| **Environment variables**           | Same env vars as in dbt platform automatically                                                                           | Use a `.env` file at the project root                                                                                             |
| **Version management**              | `dbt system update` to stay current                                                                                      | Dev container recommended for automatic sync                                                                                      |
| **dbt Mesh / deferral**             | Auto-enabled when [`dbt_cloud.yml`](../reference/dbt_cloud.yml.md) present; `--no-defer` to disable | Auto-enabled when [`dbt_cloud.yml`](../reference/dbt_cloud.yml.md) present; toggle off in extension settings |

## Related docs

* [Install dbt v2](../docs/local/install-dbt.md)
* [dbt platform CLI installation](../docs/platform/dbt-cli-installation.md)
* [dbt extension settings, including `dbt.fusionPath`](../docs/configure-dbt-extension.md#dbt-extension-settings)
* [`dbt environment` command](../reference/commands/dbt-environment.md)
* [dbt v2 releases and release channels](../docs/dbt/dbt-releases.md)
* [About profiles.yml](../docs/local/profiles.yml.md)
* [Environment variables (local)](../docs/local/configure-environment-variables.md)
* [VS Code dev containers](https://code.visualstudio.com/docs/devcontainers/containers)
* [dbt Mesh overview](../docs/mesh/about-mesh.md)
* [Deferral in dbt](../docs/platform/about-defer.md)
