# About dbt local installations

dbt enables data teams to transform data using analytics engineering best practices. You can run dbt locally through a command line interface (CLI) to build, test, and deploy your data transformations, and install additional tools to enhance your workflows.

## dbt Fusion engine[​](#dbt-fusion-engine "Direct link to dbt Fusion engine")

For the best local development experience, we recommend the dbt Fusion engine. Built in Rust, Fusion delivers:

* **Faster performance:** Up to 10x faster parsing, compilation, and execution.
* **SQL comprehension:** Dialect-aware validation catches errors before they reach your warehouse.
* **Column-level lineage:** Trace data flow across your entire project.

[Install Fusion now!](https://docs.getdbt.com/docs/local/install-dbt.md)

### dbt VS Code extension[​](#dbt-vs-code-extension "Direct link to dbt VS Code extension")

The [dbt VS Code extension](https://docs.getdbt.com/docs/dbt-extension-features.md) combines Fusion's performance with powerful LSP editor features:

* **IntelliSense:** Autocomplete for models, macros, and columns.
* **Inline errors:** See SQL errors as you type.
* **Hover insights:** View model definitions and column info without leaving your code.
* **Refactoring tools:** Rename models and columns across your project.

This is the fastest way to get started with dbt locally.

[Install Fusion with the dbt VS Code extension](https://docs.getdbt.com/docs/local/install-dbt.md)

## dbt Core[​](#dbt-core "Direct link to dbt Core")

dbt Core is the open-source engine for running dbt locally. It is available in two versions:

* [dbt Core v1](https://docs.getdbt.com/docs/local/install-dbt.md?version=1): The original Python-based dbt engine with a rich set of features.
* [dbt Core v2](https://docs.getdbt.com/docs/dbt-versions/core-upgrade/upgrading-to-v2.md) alpha: The next major version, built on the Fusion runtime. Currently in alpha.

[dbt Core v1](https://docs.getdbt.com/docs/local/install-dbt.md?version=1) includes:

* **Apache License 2.0:** dbt Core is open source now and forever.
* **Community adapters:** An amazing community of contributors has built adapters for a vast [catalog of data warehouses](https://docs.getdbt.com/docs/supported-data-platforms.md).
* **Code editor support:** Build your dbt project in popular editors like VS Code or Cursor.
* **Command line interface:** Run your project from the terminal using macOS Terminal, iTerm, or the integrated terminal in your code editor.

[Install dbt Corenow!](https://docs.getdbt.com/docs/local/install-dbt.md)

## dbt Wizard[​](#dbt-wizard "Direct link to dbt Wizard")

[dbt Wizard](https://docs.getdbt.com/docs/dbt-ai/wizard-quickstart.md) is a natural next step for local dbt development. It works with both dbt Fusion engine and dbt Core. dbt Wizard adds an AI agent that understands your full project through dbt's [native metadata engine](https://docs.getdbt.com/docs/dbt-ai/about-dbt-ai.md), a structured index of your [lineage](https://docs.getdbt.com/docs/explore/explore-projects.md), model health, test coverage, and semantic definitions.

* **Build and refactor from natural language**: Describe the change, get a reviewable diff, approve before anything is written.
* **Validate in a tight loop**: Every proposed change compiles and runs against your warehouse, catching issues before production.
* **Navigate with full project context**: Traverse the [DAG](https://docs.getdbt.com/docs/explore/explore-projects.md), surface downstream impact, and keep tests and YAML in sync as models evolve.

For data practitioners, dbt Wizard adds an AI layer that knows your project, not just your code. Refer to the [dbt Wizard quickstart](https://docs.getdbt.com/docs/dbt-ai/wizard-quickstart.md) to get started.

## dbt MCP server[​](#dbt-mcp-server "Direct link to dbt MCP server")

The dbt MCP server connects your local dbt project to AI assistants using the [Model Context Protocol](https://modelcontextprotocol.io/). It works with both dbt Fusion engine and dbt Core, and requires no repository clone.

* **dbt CLI tools:** Run `dbt run`, `build`, `test`, `compile`, `list`, `parse`, and `show` directly from your AI assistant's chat interface.
* **Local project context:** Surface model lineage, node details, and dependency graphs from your local `manifest.json` without leaving your editor.
* **Code generation:** Auto-generate model YAML, source definitions, and staging SQL from your warehouse schema (requires the codegen toolset to be enabled).
* **Zero-clone install:** Install [uv](https://docs.astral.sh/uv/getting-started/installation/) and run `uvx dbt-mcp`. No repository clone needed!

[Connect dbt MCP server to your local project](https://docs.getdbt.com/docs/dbt-ai/mcp-quickstart-cli.md)
