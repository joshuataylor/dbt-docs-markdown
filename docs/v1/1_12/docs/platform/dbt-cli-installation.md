# Install the dbt platform CLI

dbt platform

The dbt platform natively supports developing using a command line interface (CLI), empowering team members to contribute with enhanced flexibility and collaboration. The dbt platform CLI allows you to run dbt commands against your dbt platform development environment from your local command line.

CLI compatibility

The dbt platform CLI is a dbt platform tool available to users on any [plan](https://www.getdbt.com/pricing). It is intended for use only with the dbt platform and may conflict with self-hosted installations of the dbt Core or dbt Fusion engine CLIs. Refer to the [FAQs](#faqs) for more information.

dbt commands run against the platform's infrastructure and benefit from:

* Secure credential storage in the dbt platform
* [Automatic deferral](./about-defer.md) of build artifacts to your project's production environment
* Speedier, lower-cost builds
* Support for dbt Mesh ([cross-project `ref`](../mesh/govern/project-dependencies.md))

![Diagram of how the dbt platform CLI works with dbt's infrastructure to run dbt commands from your local command line.](/img/docs/dbt-platform/dbt-cli-overview.png?v=2 "Diagram of how the dbt platform CLI works with dbt's infrastructure to run dbt commands from your local command line.")Diagram of how the dbt platform CLI works with dbt's infrastructure to run dbt commands from your local command line.

## Prerequisites

The dbt platform CLI is available in all [deployment regions](./about-platform/access-regions-ip-addresses.md) and for both multi-tenant and single-tenant accounts.

* If you installed the dbt Core CLI in a virtual environment, deactivate that environment or create an alias for the platform CLI before you run platform CLI commands.
* If you installed dbt locally, create an alias for the platform CLI before you run platform CLI commands.

Refer to the [FAQs](#faqs) for more information about managing multiple dbt platform CLI tools and creating an alias.

Using the dbt platform CLI for hybrid development with Fusion?

See the [Hybrid development with dbt platform and Fusion](../../guides/dbt-platform-local-workflow.md) guide to keep credentials, environment variables, and Fusion versions in sync across your local CLI and dbt platform.

## Install dbt platform CLI

You can install the dbt platform CLI via the command line by using one of the following methods:

### macOS (brew)

Before you begin, make sure you have [Homebrew installed](http://brew.sh/) in your code editor or command line terminal. Refer to the [FAQs](#faqs) if your operating system runs into path conflicts.

1. Verify that you don't already have dbt Core installed by running the following command:

```bash
which dbt
```

If the output is `dbt not found`, then that confirms you don't have it installed.

Run `pip uninstall dbt` to uninstall dbt Core

If you've installed dbt Core globally in some other way, uninstall it first before proceeding:

```bash
pip uninstall dbt
```

2. Install the dbt platform CLI with Homebrew:

   * First, remove the `dbt-labs` tap, the separate repository for packages, from Homebrew. This prevents Homebrew from installing packages from that repository:

     ```bash
     brew untap dbt-labs/dbt
     ```

   * Then, add and install the dbt platform CLI as a package:

     ```bash
     brew tap dbt-labs/dbt-cli
     brew install dbt
     ```

     If you have multiple taps, use `brew install dbt-labs/dbt-cli/dbt`.

3. Verify your installation by running `dbt --help` in the command line. If you see the following output, you installed it correctly:

   ```bash
   The dbt CLI - an ELT tool for running SQL transformations and data models in dbt...
   ```

   If you don't see this output, check that you've deactivated pyenv or venv and don't have a global dbt version installed.

   * Note that you no longer need to run the `dbt deps` command when your environment starts. Previously, initialization required this step. However, you should still run `dbt deps` if you make any changes to your `packages.yml` file.

4. Clone your repository to your local computer using `git clone`. For example, to clone a GitHub repo using HTTPS format, run `git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY`.

5. After cloning your repo, [configure](./configure-dbt-cli.md) the dbt platform CLI for your dbt project. This lets you run dbt commands like [`dbt environment show`](../../reference/commands/dbt-environment.md?version=2.0) to view your dbt configuration or `dbt compile` to compile your project and validate models and tests. You can also add, edit, and synchronize files with your repo.

### Windows (native executable)

Refer to the [FAQs](#faqs) if your operating system runs into path conflicts.

1. Download the latest Windows release for your platform from [GitHub](https://github.com/dbt-labs/dbt-cli/releases).

2. Extract the `dbt.exe` executable into the same folder as your dbt project.

info

Advanced users can configure multiple projects to use the same dbt platform CLI by:

1. Placing the executable file (`.exe`) in the "Program Files" folder
2. [Adding it to their Windows PATH environment variable](https://medium.com/@kevinmarkvi/how-to-add-executables-to-your-path-in-windows-5ffa4ce61a53)
3. Saving it where needed

Note that if you're using VS Code, you must restart it to pick up modified environment variables.

4. Verify your installation by running `./dbt --help` in the command line. If you see the following output, you installed it correctly:

   ```bash
   The dbt CLI - an ELT tool for running SQL transformations and data models in dbt...
   ```

   If you don't see this output, check that you've deactivated pyenv or venv and don't have a global dbt version installed.

   * Note that you no longer need to run the `dbt deps` command when your environment starts. Previously, initialization required this step. However, you should still run `dbt deps` if you make any changes to your `packages.yml` file.

5. Clone your repository to your local computer using `git clone`. For example, to clone a GitHub repo using HTTPS format, run `git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY`.

6. After cloning your repo, [configure](./configure-dbt-cli.md) the dbt platform CLI for your dbt project. This lets you run dbt commands like [`dbt environment show`](../../reference/commands/dbt-environment.md?version=2.0) to view your dbt configuration or `dbt compile` to compile your project and validate models and tests. You can also add, edit, and synchronize files with your repo.

### Linux (native executable)

Refer to the [FAQs](#faqs) if your operating system runs into path conflicts.

1. Download the latest Linux release for your platform from [GitHub](https://github.com/dbt-labs/dbt-cli/releases). (Pick the file based on your CPU architecture)

2. Extract the `dbt-cli` binary to the same folder as your dbt project.

```bash
tar -xf dbt_0.29.9_linux_amd64.tar.gz
./dbt --version
```

info

Advanced users can configure multiple projects to use the same dbt platform CLI executable by adding it to their PATH environment variable in their shell profile.

3. Verify your installation by running `./dbt --help` in the command line. If you see the following output, you installed it correctly:

   ```bash
   The dbt CLI - an ELT tool for running SQL transformations and data models in dbt...
   ```

   If you don't see this output, check that you've deactivated pyenv or venv and don't have a global dbt version installed.

   * Note that you no longer need to run the `dbt deps` command when your environment starts. Previously, initialization required this step. However, you should still run `dbt deps` if you make any changes to your `packages.yml` file.

4. Clone your repository to your local computer using `git clone`. For example, to clone a GitHub repo using HTTPS format, run `git clone https://github.com/YOUR-USERNAME/YOUR-REPOSITORY`.

5. After cloning your repo, [configure](./configure-dbt-cli.md) the dbt platform CLI for your dbt project. This lets you run dbt commands like [`dbt environment show`](../../reference/commands/dbt-environment.md?version=2.0) to view your dbt configuration or `dbt compile` to compile your project and validate models and tests. You can also add, edit, and synchronize files with your repo.

## Update dbt platform CLI

The following instructions explain how to update the dbt platform CLI to the latest version depending on your operating system.

### macOS (brew)

To update the dbt platform CLI, run `brew update` and then `brew upgrade dbt`.

### Windows (executable)

To update, follow the [Windows installation instructions](./dbt-cli-installation.md?install=windows#install-dbt-cloud-cli) and replace the existing `dbt.exe` executable with the new one.

### Linux (executable)

To update, follow the [Linux installation instructions](./dbt-cli-installation.md?install=linux#install-dbt-cloud-cli) and replace the existing `dbt` executable with the new one.

## Considerations

The dbt platform CLI doesn't currently support relative paths in the [`packages.yml` file](../build/packages.md). Instead, use the [Studio IDE](./studio-ide/develop-in-studio.md), which supports relative paths in this scenario.

Here's an example of a [local package](../build/packages.md#local-packages) configuration in the `packages.yml` that won't work with the dbt platform CLI:

```yaml
# repository_root/my_dbt_project_in_a_subdirectory/packages.yml

packages:
  - local: ../shared_macros
```

In this example, `../shared_macros` is a relative path that tells dbt to look for:

* `..` — Go one directory up (to `repository_root`).
* `/shared_macros` — Find the `shared_macros` folder in the root directory.

To work around this limitation, use the [Studio IDE](./studio-ide/develop-in-studio.md), which fully supports relative paths in `packages.yml`.

## dbt Wizard

[dbt Wizard](../dbt-ai/wizard-quickstart.md) is an AI agent purpose-built for analytics engineering. It's grounded in your dbt project through a [native metadata engine](../dbt-ai/wizard-how-it-works.md#native-metadata-engine) — a structured context index of your project's lineage, model health, test coverage, and semantic definitions. Before the agent writes a single line, it knows which models are healthy, what depends on what, and where gaps in tests or documentation exist.

* **Build and refactor from natural language**: Describe the change, get a reviewable diff before anything is written to disk.
* **Validate in a tight loop**: Every proposed change compiles and runs against your warehouse, catching issues before they reach production.
* **Navigate with full project context**: Traverse the [DAG](../explore/explore-projects.md), surface downstream impact, and flag affected models, tests, and metrics before acting.

For data practitioners working with a self-hosted installation, this means AI assistance grounded in your actual project state ‐ not a generic code assistant. Bring your own key to experience the full agentic analytics engineering loop. Refer to the [dbt Wizard quickstart](../dbt-ai/wizard-quickstart.md) to get started.

## FAQs

 What's the difference between the dbt platform CLI and dbt Core?

The dbt platform CLI and [dbt Core](https://github.com/dbt-labs/dbt-core), an open-source project, are both command line tools that enable you to run dbt commands.

The key distinction is that the dbt platform CLI is tailored for the dbt platform's infrastructure and integrates with all its [features](https://docs.getdbt.com/docs/platform/about-platform/dbt-platform-features).

 How do I run both the dbt platform CLI and dbt Core?

For compatibility, both the dbt platform CLI and dbt Core are invoked by running `dbt`. This can create path conflicts if your operating system selects one over the other based on your $PATH environment variable (settings).

If you have dbt Core installed locally, either:

1. Install natively, ensuring you either deactivate the virtual environment containing dbt Core or create an alias for the dbt platform CLI.
2. (Advanced users) Install natively, but modify the $PATH environment variable to correctly point to the dbt platform CLI binary to use both dbt platform CLI and dbt Core together.

You can always uninstall the dbt platform CLI to return to using dbt Core.

 How to create an alias?

To create an alias for the dbt platform CLI:

1. Open your shell's profile configuration file. Depending on your shell and system, this could be `~/.bashrc`, `~/.bash_profile`, `~/.zshrc`, or another file.

2. Add an alias that points to the dbt platform CLI binary. For example: `alias dbt-cli="path_to_dbt_cli_binary"`

   Replace `path_to_dbt_cli_binary` with the actual path to the dbt platform CLI binary, which is `/opt/homebrew/bin/dbt`. With this alias, you can use the command `dbt-cli` to invoke the dbt platform CLI.

3. Save the file and then either restart your shell or run `source` on the profile file to apply the changes. For example, in bash you would run: `source ~/.bashrc`

4. Test and use the alias to run commands:

   * To run the dbt platform CLI, use the `dbt-cli` command: `dbt-cli command_name`. Replace 'command\_name' with the specific dbt command you want to execute.
   * To run dbt Core, use the `dbt` command: `dbt command_name`. Replace 'command\_name' with the specific dbt command you want to execute.

You can then use the `dbt-cli` command to invoke the dbt platform CLI while you keep dbt Core installed natively.

 Why am I receiving a \`Stuck session\` error when trying to run a new command?

The dbt platform CLI allows only one command that writes to the data warehouse at a time. If you attempt to run multiple write commands simultaneously (for example, `dbt run` and `dbt build`), you will encounter a `stuck session` error. To resolve this, cancel the specific invocation by passing its ID to the cancel command. For more information, refer to [parallel execution](../../reference/dbt-commands.md#parallel-execution).

 I'm getting a \`Session occupied\` error in dbt platform CLI?

If you're receiving a `Session occupied` error in the dbt platform CLI or if you're experiencing a long-running session, you can use the `dbt invocation list` command in a separate terminal window to view the status of your active session. This helps debug the issue and identify the arguments that are causing the long-running session.

To cancel an active session, use the `Ctrl + Z` shortcut.

To learn more about the `dbt invocation` command, see the [dbt invocation command reference](../../reference/commands/invocation.md?version=2.0).

Alternatively, you can reattach to your existing session with `dbt reattach` and then press `Control-C` and choose to cancel the invocation.
