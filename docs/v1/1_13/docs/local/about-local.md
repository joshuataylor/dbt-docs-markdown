# About dbt self-hosted installations

Local development

You can run dbt locally from your terminal with the dbt CLI, or from your code editor with the dbt VS Code extension. Local development lets you build, test, and run dbt projects from your own machine while connecting to your data platform.

(Applies to dbt v1.99 and earlier)

Ready for the current version?

v2 is the current generation of dbt and the recommended choice for most users — it's faster, adds richer developer tooling, and is free to use with dbt v2. [Upgrade to v2](../dbt-versions/dbt-upgrade/upgrading-to-v2.md).

## Install dbt v1

dbt v1 is the Python-based distribution and remains maintained. For full installation instructions, refer to [Install dbt](./install-dbt.md?version=1.0).

## dbt VS Code extension

The [dbt VS Code extension](../about-dbt-extension.md) lets you develop dbt projects from VS Code, Cursor, or Windsurf. Use the extension if you want an editor-based local development experience. For installation and setup, refer to the [extension docs](../about-dbt-extension.md).

## dbt Wizard

[dbt Wizard](../dbt-ai/wizard-quickstart.md) is a natural next step for local dbt development. It works with dbt and adds an AI agent that understands your full project through dbt's [native metadata engine](../dbt-ai/about-dbt-ai.md), a structured index of your [lineage](../explore/explore-projects.md), model health, test coverage, and semantic definitions.

* **Build and refactor from natural language:** Describe the change, get a reviewable diff, approve before anything is written.
* **Validate in a tight loop:** Every proposed change compiles and runs against your warehouse, catching issues before production.
* **Navigate with full project context:** Traverse the [DAG](../explore/explore-projects.md), surface downstream impact, and keep tests and YAML in sync as models evolve.

For data practitioners, dbt Wizard adds an AI layer that knows your project, not just your code. Refer to the [dbt Wizard quickstart](../dbt-ai/wizard-quickstart.md) to get started.

## dbt MCP server

The dbt MCP server connects your local dbt project to AI assistants using the [Model Context Protocol](https://modelcontextprotocol.io/). It works with dbt and requires no repository clone.

* **dbt platform CLI tools:** Run `dbt run`, `build`, `test`, `compile`, `list`, `parse`, and `show` directly from your AI assistant's chat interface.
* **Local project context:** Surface model lineage, node details, and dependency graphs from your local `manifest.json` without leaving your editor.
* **Code generation:** Auto-generate model YAML, source definitions, and staging SQL from your warehouse schema (requires the codegen toolset to be enabled).
* **Zero-clone install:** Install [uv](https://docs.astral.sh/uv/getting-started/installation/) and run `uvx dbt-mcp`. No repository clone needed!

[Connect dbt MCP server to your local project](../dbt-ai/mcp-quickstart-cli.md).

## Licensing info

dbt framework has two distributions which can both be installed locally for free, powered by a single engine:

* The Apache 2.0 licensed open-source distribution. Both v1 and v2 are available as open source installations.
* dbt v2 extends the dbt OSS offering with additional advanced capabilities — all free to use!

Refer to [licensing](../dbt-licensing.md) for more info.
