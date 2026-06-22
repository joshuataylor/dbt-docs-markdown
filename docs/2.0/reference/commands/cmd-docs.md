# About dbt docs commands

The dbt Fusion engine uses the `--write-catalog` flag instead of the `dbt docs generate` command for generating your [`catalog.json`](https://docs.getdbt.com/reference/artifacts/catalog-json.md) file and hydrating metadata. When you use `dbt build --write-catalog`, you're using a flag that performs better because it's built for the Fusion engine. To see the latest metadata in Catalog, run a job in dbt platform which uploads the metadata.

## --write-catalog flag[​](#--write-catalog-flag "Direct link to --write-catalog flag")

The `--write-catalog` flag generates the [`catalog.json`](https://docs.getdbt.com/reference/artifacts/catalog-json.md) artifact, which contains metadata about the tables and views produced by the models in your project. Fusion jobs running in dbt platform, dbt automatically runs `write-catalog`, `build`, and `run`, and hydrates your Catalog, so you don't need to manually include it. You can use this flag with the following commands:

* `dbt build`
* `dbt run`
* `dbt parse`
* `dbt compile`

**Examples**:

```shell
dbt build --write-catalog
```

### Platform behavior[​](#platform-behavior "Direct link to Platform behavior")

In dbt platform jobs running on Fusion, you don't need to change anything. When `dbt docs generate` is called (either as a job step or separate command), the platform automatically uses `--write-catalog` instead. Additionally, for Fusion jobs running in the platform, dbt will run `write-catalog` automatically with `build` or `run`, so you don't need to run a separate command to hydrate your metadata. In the platform, you can optionally choose to include it when running `dbt parse` or `dbt compile`.

Note:

### Local usage[​](#local-usage "Direct link to Local usage")

When running Fusion locally, add the `--write-catalog` flag to your command to generate the catalog:

```shell
dbt build --write-catalog
```

### What's different from docs generate[​](#whats-different-from-docs-generate "Direct link to What's different from docs generate")

The `--write-catalog` flag focuses solely on metadata hydration, generating the `catalog.json` file that powers [Catalog](https://docs.getdbt.com/docs/explore/build-and-view-your-docs.md) and metadata APIs. It does not generate the static documentation website files (`index.html`).

## dbt Docs v2 alpha[​](#dbt-docs-v2- "Direct link to dbt-docs-v2-")

The dbt Fusion engine and dbt Core v2 deliver a new version of `dbt docs serve` that powers [dbt Docs v2](https://docs.getdbt.com/docs/build/view-documentation.md#dbt-docs-v2).

Instead of loading a static `manifest.json` in the browser, v2 builds a compact binary index of your project and serves it through a local HTTP server with a REST API. This makes the experience fast even for large projects, and makes metadata queryable by AI agents and external tooling.

### Generate the index[​](#generate-the-index "Direct link to Generate the index")

Before serving, build your project with the `--write-index` flag. You can add this flag to dbt `build`, `run`, `parse`, or `compile` commands. It writes index files to the `target/index/` directory which is what `dbt docs serve` reads from:

```shell
dbt compile --write-index
```

```shell
dbt build --write-index
```

Add [`--static-analysis strict`](https://docs.getdbt.com/docs/fusion/new-concepts.md) to for column lineage and richer column metadata from your warehouse:

```shell
dbt build --write-index --static-analysis strict
```

```shell
dbt build --write-index --static-analysis strict
```

### Serve dbt Docs v2[​](#serve-dbt-docs-v2 "Direct link to Serve dbt Docs v2")

Login for full capabilities

When using Fusion, run `dbt login` before serving to unlock all capabilities. Some features, such as column lineage, require authentication to display.

Once the index is built, start the local documentation server:

```shell
dbt docs serve
```

You can pass the `--target-path` flag to change the path where dbt pulls artifacts from:

```shell
dbt docs serve --target-path ~/Developer/internal-analytics/target
```

The server starts on port `8580` by default and opens in your browser. Use `--port` to change the port:

```shell
dbt docs serve --port 8081
```

### REST API[​](#rest-api "Direct link to REST API")

dbt Docs v2 exposes a REST API at `/api/v1/` that AI agents, MCP servers, and external tooling can query directly, all without a browser. Key endpoints include:

| Endpoint                               | Description                                         |
| -------------------------------------- | --------------------------------------------------- |
| `GET /api/v1/health`                   | Server status                                       |
| `GET /api/v1/capabilities`             | Feature flags (for example, `has_column_lineage`)   |
| `GET /api/v1/models`                   | Paginated model list with filters                   |
| `GET /api/v1/models/:id`               | Model detail including catalog metadata             |
| `GET /api/v1/sources/:id`              | Source detail                                       |
| `GET /api/v1/nodes/counts`             | Resource type counts (models, sources, tests, etc.) |
| `GET /api/v1/nodes/:id/lineage`        | Model-level lineage graph                           |
| `GET /api/v1/nodes/:id/column-lineage` | Column-level lineage (Fusion-only capability)       |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

See the [dbt Docs v2 API contracts](https://github.com/dbt-labs/fs/blob/main/fs/sa/crates/dbt-docs-server/API-CONTRACTS.md#get-apiv1sources) for the full list of available endpoints.

This makes dbt Docs v2 a natural context source for MCP servers. If you're using a coding agent like Claude Code, you can point it at a running dbt Docs v2 instance to give it rich, structured metadata about your dbt project without installing dbt locally.

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
