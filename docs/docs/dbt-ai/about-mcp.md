# About dbt Model Context Protocol (MCP)

As AI becomes more deeply integrated into data workflows, dbt users need a seamless way to access and integrate dbt's structured metadata and execution context effectively. This page provides an overview of dbt's MCP Server, which exposes this context, supporting use cases such as conversational access to data, agent-driven automation of dbt workflows, and AI-assisted development.

The [dbt Model Context Protocol (MCP) server](https://github.com/dbt-labs/dbt-mcp) provides a standardized framework that enables users to seamlessly integrate AI applications with dbt-managed data assets regardless of the underlying data platforms. This ensures consistent, governed access to models, metrics, lineage, and freshness across various AI tools.

The MCP server provides access to the dbt CLI, [API](https://docs.getdbt.com/docs/dbt-cloud-apis/overview.md), the [Discovery API](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-api.md), and [Semantic Layer](https://docs.getdbt.com/docs/use-dbt-semantic-layer/dbt-sl.md). It provides access to private APIs, text-to-SQL, and SQL execution.

For more information on MCP, have a look at [Get started with the Model Context Protocol (MCP)](https://modelcontextprotocol.io/introduction).

## Server access[​](#server-access "Direct link to Server access")

You can use the dbt MCP server in two ways: locally or remotely. Choose the setup that best fits your workflow:

### Local MCP server[​](#local-mcp-server "Direct link to Local MCP server")

The local MCP server provides the best experience for development workflows, like authoring dbt models, tests, and documentation.

The [local MCP server](https://docs.getdbt.com/docs/dbt-ai/setup-local-mcp.md) runs on your machine and requires installing `uvx` (which installs dbt-mcp locally). This option provides:

* Full access to dbt CLI commands (`dbt run`, `dbt build`, `dbt test`, and more)
* Support for dbt Core, dbt CLI, and dbt Fusion engine
* Ability to work with local dbt projects without requiring a dbt platform account
* Optional integration with dbt platform APIs for metadata discovery and Semantic Layer access

### Remote MCP server[​](#remote-mcp-server "Direct link to Remote MCP server")

The remote MCP server from dbt offers data consumption use cases without local setup.

The [remote MCP server](https://docs.getdbt.com/docs/dbt-ai/setup-remote-mcp.md) connects to the dbt platform via HTTP and requires no local installation. This option is useful when:

* You either don’t want to install, or are restricted from installing, additional software on your system.
* Your use case is primarily consumption-based (for example, querying metrics, exploring metadata, viewing lineage).

<!-- -->

info

Only [`text_to_sql`](#sql) consumes dbt Copilot credits. Other MCP tools do not.

When your account runs out of Copilot credits, the remote MCP server blocks all tools that run through it, even tools invoked from a local MCP server and [proxied](https://github.com/dbt-labs/dbt-mcp/blob/main/src/dbt_mcp/tools/toolsets.py#L24) to remote MCP (like SQL and remote Fusion tools).

If you reach your dbt Copilot usage limit, all tools will be blocked until your Copilot credits reset. If you need help, please reach out to your account manager.

## Available tools[​](#available-tools "Direct link to Available tools")

### Supported[​](#supported "Direct link to Supported")

The dbt MCP server has access to many parts of the dbt experience related to development, deployment, and discovery. Here are the categories of tools supported based on what form of the MCP server you connect to as well as detailed information on exact commands or queries available to the LLM.

| Tools              | Local | Remote |
| ------------------ | ----- | ------ |
| dbt CLI            | ✅    | ❌     |
| Semantic Layer     | ✅    | ✅     |
| SQL                | ✅    | ✅     |
| Metadata Discovery | ✅    | ✅     |
| Administrative API | ✅    | ❌     |
| Codegen Tools      | ✅    | ❌     |
| Fusion Tools       | ✅    | ✅     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Note that access to the Discovery API and the Semantic Layer API is limited depending on your [plan type](https://www.getdbt.com/pricing).

### dbt CLI commands[​](#dbt-cli-commands "Direct link to dbt CLI commands")

* `build`: Executes models, tests, snapshots, and seeds in dependency order
* `compile`: Generates executable SQL from models, tests, and analyses without running them
* `docs`: Generates documentation for the dbt project
* `list`: Lists resources in the dbt project, such as models and tests
* `parse`: Parses and validates the project's files for syntax correctness
* `run`: Executes models to materialize them in the database
* `test`: Runs tests to validate data and model integrity
* `show`: Runs a query against the data warehouse
* `get_model_lineage_dev`: Gets the lineage of a model from the local development environment
* `get_node_details_dev`: Gets details about a specific node from the local development environment

Allowing your client to utilize dbt commands through the MCP tooling could modify your data models, sources, and warehouse objects. Proceed only if you trust the client and understand the potential impact.

### Semantic Layer[​](#semantic-layer "Direct link to Semantic Layer")

To learn more about the dbt Semantic layer, click [here](https://docs.getdbt.com/docs/use-dbt-semantic-layer/dbt-sl.md).

* `list_metrics`: Retrieves all defined metrics
* `list_saved_queries`: Retrieves all saved queries
* `get_dimensions`: Gets dimensions associated with specified metrics
* `get_entities`: Gets entities associated with specified metrics
* `query_metrics`: Query metrics with optional grouping, ordering, filtering, and limiting
* `get_metrics_compiled_sql`: Returns the compiled SQL generated for specified metrics and groupings without executing the query

### Metadata Discovery[​](#metadata-discovery "Direct link to Metadata Discovery")

To learn more about the dbt Discovery API, click [here](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-api.md).

* `get_mart_models`: Gets all mart models
* `get_all_models`: Gets all models
* `get_model_details`: Gets details for a specific model
* `get_model_parents`: Gets the parent nodes of a specific model
* `get_model_children`: Gets the children models of a specific model
* `get_model_health`: Gets health signals for a specific model
* `get_model_performance`: Gets execution information for models (including tests)
* `get_all_sources`: Gets all source tables with metadata and freshness information
* `get_lineage`: Gets complete lineage (ancestors/descendants) for a dbt resource with depth control and type filtering (excludes macros by default).
* `get_source_details`: Gets details for a specific source
* `get_exposures`: Gets all exposures
* `get_exposure_details`: Gets details for a specific exposure or a list of exposures
* `get_related_models`: Uses semantic search to find dbt models that are similar to the query, even if there isn't an exact string match.
* `get_macro_details`: Gets details for a specific macro
* `get_seed_details`: Gets details for a specific seed
* `get_semantic_model_details`: Gets details for a specific semantic model
* `get_snapshot_details`: Gets details for a specific snapshot
* `get_test_details`: Gets details for a specific test

### Administrative API[​](#administrative-api "Direct link to Administrative API")

To learn more about the dbt Administrative API, click [here](https://docs.getdbt.com/docs/dbt-cloud-apis/admin-cloud-api.md).

* `list_jobs`: List all jobs in a dbt account
* `get_job_details`: Get detailed information for a specific job including configuration and settings
* `get_project_details`: Get project information for a specific dbt project
* `trigger_job_run`: Trigger a job run with optional parameter overrides like Git branch, schema, or execution parameters
* `list_jobs_runs`: List runs in an account with optional filtering by job, status, or other criteria
* `get_job_run_details`: Get comprehensive run information including execution details, steps, artifacts, and debug logs
* `cancel_job_run`: Cancel a running job to stop execution
* `retry_job_run`: Retry a failed job run to attempt execution again
* `list_job_run_artifacts`: List all available artifacts for a job run (manifest.json, catalog.json, logs, etc.)
* `get_job_run_artifact`: Download specific artifact files from job runs for analysis or integration
* `get_job_run_error`: Retrieves error details for failed job runs to help troubleshoot errors (includes option to return warning and deprecation details)

### SQL (remote)[​](#sql-remote "Direct link to SQL (remote)")

* `text_to_sql`: Generate SQL from natural language requests
* `execute_sql`: Execute SQL on the dbt platform's backend infrastructure with support for Semantic Layer SQL syntax. Note: using a PAT instead of a service token for `DBT_TOKEN` is required for this tool.

### Codegen tools[​](#codegen-tools "Direct link to Codegen tools")

These tools help automate boilerplate code generation for dbt project files. To use them, install the [dbt-codegen](https://hub.getdbt.com/dbt-labs/codegen/latest/) in your dbt project. These tools are disabled by default. To enable them, set the `DISABLE_DBT_CODEGEN` environment variable to `false`.

* `generate_source`: Creates source YAML definitions from database schemas.
* `generate_model_yaml`: Generates documentation YAML for existing dbt models, including column names, data types, and description placeholders.
* `generate_staging_model`: Creates staging SQL models from sources to transform raw source data into clean staging models.

### Fusion tools (remote)[​](#fusion-tools-remote "Direct link to Fusion tools (remote)")

A set of tools that leverage the Fusion engine for advanced SQL compilation and column-level lineage analysis.

* `compile_sql`: Compiles a SQL statement in the context of the current project and environment.
* `get_column_lineage`: Fusion exclusive! Get column lineage information across a project DAG for a specific column.

### Fusion tools (local)[​](#fusion-tools-local "Direct link to Fusion tools (local)")

A set of tools that leverage the Fusion engine through a locally running Fusion Language Server Protocol (LSP) in VS Code or Cursor with the dbt VS Code extension.

* `get_column_lineage`: Fusion exclusive! Get column lineage information across a project DAG for a specific column.

### MCP server metadata[​](#mcp-server-metadata "Direct link to MCP server metadata")

These tools provide information about the MCP server itself. They are disabled by default. To enable them, set the `DISABLE_MCP_SERVER_METADATA` environment variable to `false`.

* `get_mcp_server_version`: Returns the current version of the dbt MCP server.

## MCP integrations[​](#mcp-integrations "Direct link to MCP integrations")

The dbt MCP server integrates with any [MCP client](https://modelcontextprotocol.io/clients) that supports token authentication and tool use capabilities.

We have also created integration guides for the following clients:

* [Claude](https://docs.getdbt.com/docs/dbt-ai/integrate-mcp-claude.md)
* [Cursor](https://docs.getdbt.com/docs/dbt-ai/integrate-mcp-cursor.md)
* [VS Code](https://docs.getdbt.com/docs/dbt-ai/integrate-mcp-vscode.md)

## Resources[​](#resources "Direct link to Resources")

* For more information, refer to our blog on [Introducing the dbt MCP Server](https://docs.getdbt.com/blog/introducing-dbt-mcp-server#getting-started).

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
