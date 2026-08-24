# About dbt Model Context Protocol (MCP) server

The [dbt MCP server](https://github.com/dbt-labs/dbt-mcp) provides a standardized framework that lets you integrate AI applications with dbt‑managed data assets across different data platforms. This ensures consistent, governed access to models, metrics, lineage, and freshness across your AI tools.

To help with dbt, assistants need your project metadata and, when you allow it, supported actions such as CLI runs, platform APIs, and Semantic Layer queries. The dbt MCP server exposes those to MCP clients and supports use cases such as conversational access to data, agentic automation for dbt workflows, and AI-assisted development. This page covers local and remote setups, available tools, and how to get started.

The MCP server provides access to [dbt Wizard](../platform/wizard-overview.md), dbt platform CLI, [API](../dbt-apis/overview.md), the [Discovery API](../dbt-apis/discovery-api.md), and [Semantic Layer](../use-dbt-semantic-layer/dbt-sl.md). It provides access to private APIs, text-to-SQL, and SQL execution.

For more information on MCP, have a look at [Get started with the Model Context Protocol (MCP)](https://modelcontextprotocol.io/introduction).

The dbt MCP server comes in two flavors: self-hosted and remote.

* [Self-hosted MCP server](#local-mcp-server): runs on your machine and requires installing `uvx` (which installs dbt-mcp locally).
* [Remote MCP server](#remote-mcp-server): uses an HTTP connection and makes calls to dbt-mcp remotely on the managed dbt platform. This is a dbt-hosted solution and is ideal for data consumption use cases.

For more details on the server types, refer to [Server access](#server-access).

## Get started

To get started, choose the quickstart that matches your setup:

| I want to...                                                                                                                                                  | Quickstart                                                                                           | Tool access                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- | ------------------------------------------------- |
| Query data and run dbt platform CLI commands locally while connected to my dbt platform account (Semantic Layer, Discovery API, Admin API, SQL, Codegen).     | [Connect to dbt platform](./mcp-quickstart-oauth.md)               | Uses [self-hosted MCP server](#local-mcp-server). |
| Run dbt platform CLI commands locally, with or without a dbt platform account; with an account, also query data and explore metadata through the same server. | [Run dbt locally](./mcp-quickstart-cli.md)                         | Uses [self-hosted MCP server](#local-mcp-server). |
| Use MCP with zero installation (query data only through dbt-hosted tools; no dbt platform CLI commands).                                                      | [Connect to the remote dbt MCP server](./mcp-quickstart-remote.md) | Uses [remote MCP server](#remote-mcp-server).     |

To configure or disable specific tools (self-hosted MCP), see the [Environment variables reference](./mcp-environment-variables.md).

## Server access

You can use the dbt MCP server in the following ways:

* [Self-hosted MCP server](#local-mcp-server) — runs on your machine and requires installing `uvx` (which installs dbt-mcp locally) and then running `uvx dbt-mcp` to start the server. No need to clone the repo unless you want to contribute to [dbt MCP server](https://github.com/dbt-labs/dbt-mcp).
* [Remote MCP server](#remote-mcp-server) — uses an HTTP connection and makes calls to dbt-mcp hosted on the managed dbt platform. This setup requires no self-hosted installation and is ideal for data consumption use cases.

### Self-hosted MCP server

The self-hosted MCP server provides the best experience for development workflows, like authoring dbt models, tests, and documentation.

The [self-hosted MCP server](./setup-local-mcp.md) runs on your machine and requires installing `uvx` (which installs dbt-mcp locally) and then running `uvx dbt-mcp` to start the server. You don't need to clone the repository unless you want to contribute to dbt MCP. The self-hosted MCP server provides:

* Full access to dbt commands (`dbt run`, `dbt build`, `dbt test`, and more)
* Support for dbt Core, dbt platform CLI, and dbt Fusion engine
* Ability to work with local dbt projects with or without a dbt platform account
* Optional integration with dbt platform APIs for metadata discovery and Semantic Layer access

### Remote MCP server

The remote MCP server from dbt offers data consumption use cases without a self-hosted setup. It doesn't support local development or dbt platform CLI commands; use the [self-hosted MCP server](./setup-local-mcp.md) for those workflows.

The [remote MCP server](./setup-remote-mcp.md) connects to the dbt platform via HTTP and requires no installation. This option is useful when:

* You either don’t want to install, or are restricted from installing, additional software on your system.
* Your use case is primarily consumption-based (for example, querying metrics, exploring metadata, viewing lineage).

The remote MCP server is available on all dbt platform [plans](https://www.getdbt.com/pricing). However, the underlying [dbt APIs](../dbt-apis/overview.md) that the server's tools rely on vary by plan type. For example, the Discovery API and Semantic Layer APIs. As a result, the tools available to you through the remote MCP server depend on your plan.

Remote MCP uses the same default global API rate limit as other dbt APIs (5,000 requests per minute per IP). Self-hosted MCP is subject to the public Administrative and Discovery API limits. Refer to [API rate limits](../dbt-apis/rate-limits.md) for details.

info

Only [`text_to_sql`](./mcp-available-tools.md) consumes your dbt Copilot action allotment. Other MCP tools do not.

When your account runs out of dbt Copilot actions, the remote MCP server blocks every tool that runs through it, including tools invoked from a self-hosted MCP server and [proxied](https://github.com/dbt-labs/dbt-mcp/blob/main/src/dbt_mcp/tools/toolsets.py#L24) to remote MCP, such as SQL and remote Fusion tools.

If you reach your dbt Copilot actions limit, remote MCP tools remain unavailable until the limit resets. If you need help, contact your account manager.

### Supported tools by MCP server type

The dbt MCP server has access to many parts of the dbt experience related to development, deployment, and discovery. Here are the categories of tools supported based on what form of the MCP server you connect to as well as detailed information on exact commands or queries available to the LLM.

Self-hosted MCP is required for dbt platform CLI commands, Codegen, and Administrative API; remote MCP supports Semantic Layer, SQL, Discovery, Administrative API, and Fusion tools only.

Note that access to the [dbt APIs](../dbt-apis/overview.md) is limited depending on your [plan type](https://www.getdbt.com/pricing).

| Tools                     | Self-hosted | Remote |
| ------------------------- | ----------- | ------ |
| dbt platform CLI commands | ✅          | ❌     |
| Semantic Layer            | ✅          | ✅     |
| SQL                       | ✅          | ✅     |
| Metadata Discovery        | ✅          | ✅     |
| Administrative API        | ✅          | ✅     |
| Codegen Tools             | ✅          | ❌     |
| Fusion Tools              | ✅          | ✅     |
| Product Docs Tools        | ✅          | ✅     |
| MCP Server Metadata Tools | ✅          | ✅     |

## Available tools

The dbt MCP server has access to many tools related to development, deployment, and discovery — like CLI

A full list of tools is available for your MCP server and is auto-fetched from the [dbt MCP server README on GitHub](https://github.com/dbt-labs/dbt-mcp#tools) when the docs are built, so it stays in sync with each release.

To view the full list of tools, see [Available tools](./mcp-available-tools.md).

## MCP integrations

The dbt MCP server integrates with any [MCP client](https://modelcontextprotocol.io/clients) that supports OAuth or token authentication and tool use capabilities, depending on your setup. We have created integration guides for the following clients:

* [Claude](./integrate-mcp-claude.md)
* [Cursor](./integrate-mcp-cursor.md)
* [VS Code](./integrate-mcp-vscode.md)
* [Snowflake Cortex](./integrate-mcp-snowflake-cortex.md)

## Data retention

The dbt MCP server doesn't store or retain any production data or job run results. It's a read-only access layer that reads metadata, job results, and Semantic Layer data from the dbt platform in real time when a tool is called.

Your [dbt platform data retention policy](https://www.getdbt.com/security) determines how long job runs and artifacts are available, not the MCP server. As long as a job or artifact exists in dbt platform, the MCP server can read it.

## Resources

* [Environment variables reference](./mcp-environment-variables.md) — full list of variables and tool configuration for self-hosted MCP
* For more information, refer to our blog on [Introducing the dbt MCP Server](https://docs.getdbt.com/blog/introducing-dbt-mcp-server#getting-started).
