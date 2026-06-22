# Run your dbt projects

You can run your dbt projects locally or using the [dbt platform](https://docs.getdbt.com/docs/platform/about-platform/dbt-platform-features.md) with the dbt framework.

## Common commands[​](#common-commands "Direct link to Common commands")

In dbt, the commands you commonly use are:

* [dbt run](https://docs.getdbt.com/reference/commands/run.md) — Run the models you defined in your project
* [dbt build](https://docs.getdbt.com/reference/commands/build.md) — Build and test your selected resources such as models, seeds, snapshots, and tests
* [dbt test](https://docs.getdbt.com/reference/commands/test.md) — Execute the tests you defined for your project

For all dbt commands and their arguments (flags), see the [dbt command reference](https://docs.getdbt.com/reference/dbt-commands.md). To list all dbt commands from the command line, run `dbt --help`. To list a specific command's arguments, run `dbt COMMAND_NAME --help`.

 New to the command line?

If you're new to the command line:

1. Open your computer's terminal application (such as Terminal or iTerm) to access the command line.
2. Make sure you navigate to your dbt project directory before running any dbt commands.
3. These terminal commands help you navigate your file system: `cd` (change directory), `ls` (list directory contents), and `pwd` (present working directory).

## Where to run dbt[​](#where-to-run-dbt "Direct link to Where to run dbt")

<!-- -->

Use the dbt framework to quickly and collaboratively transform data and deploy analytics code following software engineering best practices like version control, modularity, portability, CI/CD, and documentation. This means anyone on the data team familiar with SQL can safely contribute to production-grade data pipelines.

The dbt framework is composed of a *language* and an *engine*:

* The *dbt language* is the code you write in your dbt project — SQL select statements, Jinja templating, YAML configs, tests, and more. It's the standard for the data industry and the foundation of the dbt framework.

* The *dbt engine* compiles your project, executes your transformation graph, and produces metadata. dbt supports two engines which you can use depending on your needs:

  * The dbt Core engine, which renders Jinja and runs your models.
  * The dbt Fusion engine, which goes beyond Jinja rendering to statically analyze your SQL — validating syntax and logic before your SQL is sent to the database (saving compute resources), and supports LSP features.

### dbt platform[​](#dbt-platform "Direct link to dbt platform")

The dbt platform is a fully managed service that gives you a complete environment to build, test, deploy, and collaborate on dbt projects. You can develop in the browser or locally using the dbt Fusion engine or dbt Core engine.

* [Develop in your browser using the Studio IDE](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md)
* [Seamless drag-and-drop development with Canvas](https://docs.getdbt.com/docs/platform/canvas.md)
* [Run dbt commands from your local command line](#dbt-local-development) using dbt VS Code extension or dbt CLI (both which integrate seamlessly with the dbt platform project(s)).

For more details, see [About dbt plans](https://www.getdbt.com/pricing).

### dbt local development[​](#dbt-local-development "Direct link to dbt local development")

You can run dbt locally with the dbt Fusion engine or the dbt Core engine:

* [Install the dbt VS Code extension](https://docs.getdbt.com/docs/about-dbt-extension.md) — Combines dbt Fusion engine performance with visual features like autocomplete, inline errors, and lineage. Includes [LSP features](https://docs.getdbt.com/docs/about-dbt-lsp.md) and suitable for users with dbt platform projects or running dbt locally without a dbt platform project. *Recommended for local development.*
* [Install the Fusion CLI](https://docs.getdbt.com/docs/local/install-dbt.md?version=2) — dbt Fusion engine from the command line, but doesn't include LSP features.
* [Install the dbt CLI](https://docs.getdbt.com/docs/platform/dbt-cli-installation.md) — The dbt platform CLI, which allows you to run dbt commands against your dbt platform development environment from your local command line. Requires a dbt platform project.
* [Install dbt Core v1](https://docs.getdbt.com/docs/local/install-dbt.md) — The open-source, Python-based CLI that uses the dbt Core v1 engine. Doesn't include LSP features.
* [Install dbt Core v2](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md)alpha — The open-source, Rust-based CLI built on the Fusion runtime. Currently in alpha. Doesn't include LSP features.

## Related docs[​](#related-docs "Direct link to Related docs")

* [About the dbt VS Code extension](https://docs.getdbt.com/docs/about-dbt-extension.md)
* [dbt features](https://docs.getdbt.com/docs/platform/about-platform/dbt-platform-features.md)
* [Model selection syntax](https://docs.getdbt.com/reference/node-selection/syntax.md)
* [dbt CLI](https://docs.getdbt.com/docs/platform/dbt-cli-installation.md)
* [Studio IDE features](https://docs.getdbt.com/docs/platform/studio-ide/develop-in-studio.md#studio-ide-features)
* [Does dbt offer extract and load functionality?](https://docs.getdbt.com/faqs/Project/transformation-tool.md)
* [Why does dbt compile need a data platform connection](https://docs.getdbt.com/faqs/Warehouse/db-connection-dbt-compile.md)

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
