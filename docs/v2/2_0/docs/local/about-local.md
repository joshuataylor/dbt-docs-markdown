# About dbt self-hosted installations

Local developmentⓘ

You can run dbt locally from your terminal with the dbt CLI, or from your code editor with the dbt VS Code extension. Local development lets you build, test, and run dbt projects from your own machine while connecting to your data platform.

## Install dbt[​](#install-dbt "Direct link to Install dbt")

Install dbt locally to run dbt commands from your terminal. You can install it with `pip`:

```shell
python -m pip install --pre dbt-<adapter>
```

Or with `curl`:

```shell
curl -fsSL https://public.cdn.getdbt.com/fs/install/install.sh | sh
```

For full installation instructions, including adapter-specific setup, refer to [Install dbt](https://docs.getdbt.com/docs/local/install-dbt.md).

<!-- -->

## dbt VS Code extension[​](#dbt-vs-code-extension "Direct link to dbt VS Code extension")

The [dbt VS Code extension](https://docs.getdbt.com/docs/about-dbt-extension.md) lets you develop dbt projects from VS Code, Cursor, or Windsurf. Use the extension if you want an editor-based local development experience. For installation and setup, refer to the [extension docs](https://docs.getdbt.com/docs/about-dbt-extension.md).

## dbt Wizard[​](#dbt-wizard "Direct link to dbt Wizard")

[dbt Wizard](https://docs.getdbt.com/docs/dbt-ai/wizard-quickstart.md) is a natural next step for local dbt development. It works with dbt and adds an AI agent that understands your full project through dbt's [native metadata engine](https://docs.getdbt.com/docs/dbt-ai/about-dbt-ai.md), a structured index of your [lineage](https://docs.getdbt.com/docs/explore/explore-projects.md), model health, test coverage, and semantic definitions.

* **Build and refactor from natural language:** Describe the change, get a reviewable diff, approve before anything is written.
* **Validate in a tight loop:** Every proposed change compiles and runs against your warehouse, catching issues before production.
* **Navigate with full project context:** Traverse the [DAG](https://docs.getdbt.com/docs/explore/explore-projects.md), surface downstream impact, and keep tests and YAML in sync as models evolve.

For data practitioners, dbt Wizard adds an AI layer that knows your project, not just your code. Refer to the [dbt Wizard quickstart](https://docs.getdbt.com/docs/dbt-ai/wizard-quickstart.md) to get started.

## dbt MCP server[​](#dbt-mcp-server "Direct link to dbt MCP server")

The dbt MCP server connects your local dbt project to AI assistants using the [Model Context Protocol](https://modelcontextprotocol.io/). It works with dbt and requires no repository clone.

* **dbt CLI tools:** Run `dbt run`, `build`, `test`, `compile`, `list`, `parse`, and `show` directly from your AI assistant's chat interface.
* **Local project context:** Surface model lineage, node details, and dependency graphs from your local `manifest.json` without leaving your editor.
* **Code generation:** Auto-generate model YAML, source definitions, and staging SQL from your warehouse schema (requires the codegen toolset to be enabled).
* **Zero-clone install:** Install [uv](https://docs.astral.sh/uv/getting-started/installation/) and run `uvx dbt-mcp`. No repository clone needed!

[Connect dbt MCP server to your local project](https://docs.getdbt.com/docs/dbt-ai/mcp-quickstart-cli.md).

## Licensing info[​](#licensing-info "Direct link to Licensing info")

dbt framework has two distributions which can both be installed locally for free, powered by a single engine:

* dbt Core is completely open-source and the code behind Fusion. Its code and binary are subject to the Apache 2.0 license.
  <!-- -->
  * Includes dbt Core v1.x and dbt Core 2.0
* dbt Fusion extends dbt Core with additional advanced capabilities — some are free to use, and other premium features (under proprietary code) are unlocked with a free login or payment method.

Refer to [licensing](https://docs.getdbt.com/docs/dbt-licensing.md) for more info.
