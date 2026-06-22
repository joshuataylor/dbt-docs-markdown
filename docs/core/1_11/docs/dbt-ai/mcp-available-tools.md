# Available tools

The dbt MCP server has access to many parts of the dbt experience related to development, deployment, and discovery. This page lists the tools available in the dbt MCP server and how to configure them.

## Supported tools by MCP server type[​](#supported-tools-by-mcp-server-type "Direct link to Supported tools by MCP server type")

The following sections list every tool by category, along with which server type (local or remote) supports it.

| Tools                             | Local | Remote |
| --------------------------------- | ----- | ------ |
| [dbt commands](#dbt-commands)     | ✅    | ❌     |
| [Semantic Layer](#semantic-layer) | ✅    | ✅     |
| [SQL](#sql)                       | ✅    | ✅     |
| [Metadata Discovery](#discovery)  | ✅    | ✅     |
| [Administrative API](#admin-api)  | ✅    | ✅     |
| [Codegen Tools](#dbt-codegen)     | ✅    | ❌     |
| [Fusion Tools](#dbt-lsp)          | ✅    | ✅     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Note that access to the Discovery API and the Semantic Layer API is limited depending on your [plan type](https://www.getdbt.com/pricing).

## dbt MCP server tool list[​](#dbt-mcp-server-tool-list "Direct link to dbt MCP server tool list")

The following tool list is available for your MCP server and is auto-fetched from the [dbt MCP server README on GitHub](https://github.com/dbt-labs/dbt-mcp#tools) when the docs are built, so it stays in sync with each release.

### SQL[​](#sql "Direct link to SQL")

Tools for executing and generating SQL on dbt Platform infrastructure.

* `execute_sql`: Executes SQL on dbt Platform infrastructure with Semantic Layer support.
* `text_to_sql`: Generates SQL from natural language using project context.

### Semantic Layer[​](#semantic-layer "Direct link to Semantic Layer")

To learn more about the dbt Semantic Layer, click [here](https://docs.getdbt.com/docs/use-dbt-semantic-layer/dbt-sl).

* `get_dimension_values`: Gets distinct values for a dimension; option to scope to specific metrics.
* `get_dimensions`: Gets dimensions for specified metrics.
* `get_entities`: Gets entities for specified metrics.
* `get_metrics_compiled_sql`: Returns compiled SQL for metrics without executing the query.
* `list_metrics`: Retrieves all defined metrics.
* `list_saved_queries`: Retrieves all saved queries.
* `query_metrics`: Executes metric queries with filtering and grouping options.

### Discovery[​](#discovery "Direct link to Discovery")

To learn more about the dbt Discovery API, click [here](https://docs.getdbt.com/docs/dbt-cloud-apis/discovery-api).

* `get_all_macros`: Retrieves macros; option to filter by package or return package names only.
* `get_all_models`: Retrieves name and description of all models.
* `get_all_sources`: Gets all sources with freshness status; option to filter by source name.
* `get_exposure_details`: Gets exposure details including owner, parents, and freshness status.
* `get_exposures`: Gets all exposures (downstream dashboards, apps, or analyses).
* `get_lineage`: Gets full lineage graph (ancestors and descendants) with type and depth filtering.
* `get_macro_details`: Gets details for a specific macro.
* `get_mart_models`: Retrieves all mart models.
* `get_model_children`: Gets downstream dependents of a model.
* `get_model_details`: Gets model details including compiled SQL, columns, and schema.
* `get_model_health`: Gets health signals: run status, test results, and upstream source freshness.
* `get_model_parents`: Gets upstream dependencies of a model.
* `get_model_performance`: Gets execution history for a model; option to include test results.
* `get_related_models`: Finds similar models using semantic search.
* `get_seed_details`: Gets details for a specific seed.
* `get_semantic_model_details`: Gets details for a specific semantic model.
* `get_snapshot_details`: Gets details for a specific snapshot.
* `get_source_details`: Gets source details including columns and freshness.
* `get_test_details`: Gets details for a specific test.
* `search`: \[Alpha] Searches for resources across the dbt project (not generally available).

### dbt CLI[​](#dbt-cli "Direct link to dbt CLI")

Allowing your client to utilize dbt commands through the MCP tooling could modify your data models, sources, and warehouse objects. Proceed only if you trust the client and understand the potential impact.

* `build`: Executes models, tests, snapshots, and seeds in DAG order.
* `clone`: Clones selected nodes from the specified state to the target schema(s).
* `compile`: Generates executable SQL from models/tests/analyses; useful for validating Jinja logic.
* `docs`: Generates documentation for the dbt project.
* `get_lineage_dev`: Retrieves lineage from local manifest.json with type and depth filtering.
* `get_node_details_dev`: Retrieves node details from local manifest.json (models, seeds, snapshots, sources).
* `list`: Lists resources in the dbt project by type with selector support.
* `parse`: Parses and validates project files for syntax correctness.
* `run`: Executes models to materialize them in the database.
* `show`: Executes SQL against the database and returns results.
* `test`: Runs tests to validate data and model integrity.

### Admin API[​](#admin-api "Direct link to Admin API")

To learn more about the dbt Administrative API, click [here](https://docs.getdbt.com/docs/dbt-cloud-apis/admin-cloud-api).

* `cancel_job_run`: Cancels a running job.
* `get_job_details`: Gets job configuration including triggers, schedule, and dbt commands.
* `get_job_run_details`: Gets run details including status, timing, steps, and artifacts.
* `get_job_run_error`: Gets error and/or warning details for a job run; option to include or show warnings only.
* `list_job_run_artifacts`: Lists available artifacts from a job run.
* `list_jobs`: Lists jobs in a dbt Platform account; option to filter by project or environment.
* `list_jobs_runs`: Lists job runs; option to filter by job, status, or order by field.
* `list_projects`: Lists all projects in the dbt Platform account.
* `retry_job_run`: Retries a failed job run.
* `trigger_job_run`: Triggers a job run; option to override git branch, schema, or other settings.

### dbt Codegen[​](#dbt-codegen "Direct link to dbt Codegen")

These tools help automate boilerplate code generation for dbt project files.

* `generate_model_yaml`: Generates model YAML with columns; option to inherit upstream descriptions.
* `generate_source`: Generates source YAML by introspecting database schemas; option to include columns.
* `generate_staging_model`: Generates staging model SQL from a source table.

### dbt LSP[​](#dbt-lsp "Direct link to dbt LSP")

A set of tools that leverage the Fusion engine for advanced SQL compilation and column-level lineage analysis.

* `fusion.compile_sql`: Compiles SQL in project context via dbt Platform.
* `fusion.get_column_lineage`: Traces column-level lineage via dbt Platform.
* `get_column_lineage`: Traces column-level lineage locally (requires dbt-lsp via dbt Labs VSCE).

### Product Docs[​](#product-docs "Direct link to Product Docs")

Tools for searching and fetching content from the official dbt documentation at docs.getdbt.com.

* `get_product_doc_pages`: Fetches the full Markdown content of one or more docs.getdbt.com pages by path or URL.
* `search_product_docs`: Searches docs.getdbt.com for pages matching a query; returns titles, URLs, and descriptions ranked by relevance. Use get\_product\_doc\_pages to fetch full content.

### MCP Server Metadata[​](#mcp-server-metadata "Direct link to MCP Server Metadata")

These tools provide information about the MCP server itself.

* `get_mcp_server_branch`: Returns the current git branch of the running dbt MCP server.
* `get_mcp_server_version`: Returns the current version of the dbt MCP server.
