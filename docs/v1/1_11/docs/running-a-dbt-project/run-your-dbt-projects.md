# Run your dbt projects

You can run your dbt projects locally or using the [dbt platform](../platform/about-platform/dbt-platform-features.md) with the dbt framework.

## Common commands

In dbt, the commands you commonly use are:

* [dbt run](../../reference/commands/run.md) — Run the models you defined in your project
* [dbt build](../../reference/commands/build.md) — Build and test your selected resources such as models, seeds, snapshots, and tests
* [dbt test](../../reference/commands/test.md) — Execute the tests you defined for your project

For all dbt commands and their arguments (flags), see the [dbt command reference](../../reference/dbt-commands.md). To list all dbt commands from the command line, run `dbt --help`. To list a specific command's arguments, run `dbt COMMAND_NAME --help`.

 New to the command line?

If you're new to the command line:

1. Open your computer's terminal application (such as Terminal or iTerm) to access the command line.
2. Make sure you navigate to your dbt project directory before running any dbt commands.
3. These terminal commands help you navigate your file system: `cd` (change directory), `ls` (list directory contents), and `pwd` (present working directory).

## Where to run dbt

Use the dbt framework to quickly and collaboratively transform data and deploy analytics code following software engineering best practices like version control, modularity, portability, CI/CD, and documentation. This means anyone on the data team familiar with SQL can safely contribute to production-grade data pipelines.

The dbt framework is composed of a *language* and an *engine*:

* The *dbt language* is the code you write in your dbt project — SQL select statements, Jinja templating, YAML configs, tests, and more. It's the standard for the data industry and the foundation of the dbt framework.

* The *dbt engine* compiles your project, executes your transformation graph, and produces metadata. Today, the current Rust-based version generation is v2. By default, installing dbt gives you SQL comprehension, editor features, and richer development workflows.

### dbt platform

The dbt platform is a fully managed service that gives you a complete environment to build, test, deploy, and collaborate on dbt projects. You can develop in the browser or with a self-hosted installation using the dbt Fusion engine or dbt Core engine.

* [Develop in your browser using the Studio IDE](../platform/studio-ide/develop-in-studio.md)
* [Seamless drag-and-drop development with Canvas](../platform/canvas.md)
* [Run dbt commands from your local command line](#dbt-local-development) using dbt VS Code extension or dbt CLI (both which integrate seamlessly with the dbt platform project(s)).

For more details, see [About dbt plans](https://www.getdbt.com/pricing).

### Self-hosted dbt development

You can run dbt locally with the dbt Fusion engine or the dbt Core engine:

* [Install dbt](../local/install-dbt.md) — Get Fusion from the command line
* [Install the dbt VS Code extension](../about-dbt-extension.md) — Combines dbt Fusion engine performance with visual features like autocomplete, inline errors, and lineage. Includes [LSP features](../about-dbt-lsp.md) and suitable for users with dbt platform projects or running dbt locally without a dbt platform project. *Recommended for local development.*
* [Install the dbt CLI](../platform/dbt-cli-installation.md) — The dbt platform CLI, which allows you to run dbt commands against your dbt platform development environment from your local command line. Requires a dbt platform project.

## Related docs

* [About the dbt VS Code extension](../about-dbt-extension.md)
* [dbt features](../platform/about-platform/dbt-platform-features.md)
* [Model selection syntax](../../reference/node-selection/syntax.md)
* [dbt CLI](../platform/dbt-cli-installation.md)
* [Studio IDE features](../platform/studio-ide/develop-in-studio.md#studio-ide-features)
* [Does dbt offer extract and load functionality?](../../faqs/Project/transformation-tool.md)
* [Why does dbt compile need a data platform connection](../../faqs/Warehouse/db-connection-dbt-compile.md)
