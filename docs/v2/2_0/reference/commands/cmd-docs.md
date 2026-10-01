# About dbt docs commands

(Applies to dbt v2.0 and later)

With dbt v2, use [dbt Docs v2](../../docs/build/view-documentation.md#dbt-docs-v2) to generate and view your project's documentation locally. Run `dbt docs generate` to build the site, then `dbt docs serve` to preview it.

To generate catalog metadata (`catalog.json`) without building the documentation site, use the [`--write-catalog` flag](#--write-catalog-flag).

dbt Docs v2 is built for local and self-hosted workflows. In dbt platform, use [Catalog](../../docs/explore/build-and-view-your-docs.md) instead, a hosted, always-up-to-date view of your docs that your jobs refresh automatically. Refer to [Platform behavior](#platform-behavior) for details.

## dbt Docs v2

dbt Docs v2 is a self-hosted documentation site generated on your local machine or your own pipeline (like GitHub Actions). It isn't available in the dbt platform, where you use [Catalog](../../docs/explore/build-and-view-your-docs.md) instead. Refer to [dbt platform behavior](#dbt-platform-behavior) for details.

Instead of loading a static `manifest.json` in the browser, v2 produces Parquet artifacts when you compile or build your project. `dbt docs generate` exports a documentation site made of plain static files (a single-page app plus those artifacts) that any file host can serve. The browser reads the Parquet directly using DuckDB-WASM (WebAssembly), so you don't need to run a stateful server to view your docs. This keeps the experience fast even for large projects.

### Generate the site

`dbt docs generate` compiles your project, writes the index, and exports the documentation site in a single command:

```shell
dbt docs generate
```

Report incorrect code

By default, dbt writes the site into your `target/` directory (`target/index.html`, `target/assets/`, and the index under `target/index/`), matching the layout of dbt v1. You can serve `index.html` from `target/` the same way you did in v1, so a self-hosted pipeline that runs `dbt docs generate && mv target public` keeps working.

Use `--output-dir` to write a self-contained copy of the site to a different directory:

```shell
dbt docs generate --output-dir site
```

Report incorrect code

This writes a `site/` directory (the app, hashed assets, and a copy of the index) that you can host on S3, GitHub Pages, Netlify, GitLab Pages, or any similar static file host.

To skip compilation and export whatever index is already on disk, use `--no-compile`, which fails with an error if no index exists:

```shell
dbt docs generate --no-compile
```

Report incorrect code

#### Column lineage and richer metadata

Column-level lineage and richer column metadata require an index built with [`--static-analysis strict`](../../docs/build/about-static-analysis.md). Because `dbt docs generate` runs a standard compile by default, build the index with strict static analysis first when you want column lineage, then export it:

```shell
dbt build --write-index --static-analysis strict
dbt docs generate --no-compile
```

Report incorrect code

If you generate the site without column lineage, dbt Docs v2 hides those features instead of showing empty data.

### Serve dbt Docs v2

To preview the site locally, run:

```shell
dbt docs serve
```

Report incorrect code

Use `dbt docs serve` to view your documentation locally on your own machine. In dbt platform, use Catalog to explore your project. Refer to [platform behavior](#platform-behavior) for details.

`dbt docs serve` generates the site if it's missing or older than the index, then serves the static files. The server starts on port `8580` by default and opens in your browser. Use `--port` to change the port:

```shell
dbt docs serve --port 8081
```

Report incorrect code

Use the `--target-path` flag to change the path where dbt reads artifacts from:

```shell
dbt docs serve --target-path ~/Developer/internal-analytics/target
```

Report incorrect code

Because the generated site is a set of static files, you can also host it on any static file host — such as cloud object storage or a static site host — instead of serving it locally.

### Project overview page

dbt Docs v2 renders your project's `__overview__` doc block as the landing page, the same as dbt Docs v1. dbt discovers overview content by scanning your `docs-paths` for `{% docs %}` blocks, so a block in `models/overview.md` is found by default. A file at `docs/overview.md` is only picked up when your project sets `docs-paths: ["docs"]`. If your project defines no overview, dbt renders its default overview content.

## --write-catalog flag

The `--write-catalog` flag generates the [`catalog.json`](../artifacts/catalog-json.md) artifact, which contains metadata about the tables and views produced by the models in your project. It focuses solely on metadata hydration and does not build the documentation site — use [dbt Docs v2](#dbt-docs-v2) for that.

When you run dbt locally, add the flag yourself. You can use it with the following commands:

* `dbt build`
* `dbt run`
* `dbt parse`
* `dbt compile`

**Example**:

```shell
dbt build --write-catalog
```

Report incorrect code

dbt platform jobs

dbt v2 jobs in dbt platform refresh Catalog metadata automatically on every run, so you don't need to add the `--write-catalog` flag there. Refer to [Platform behavior](#platform-behavior) for more info.

### Local usage

When running dbt v2 locally, add the `--write-catalog` flag to your command to generate the catalog:

```shell
dbt build --write-catalog
```

Report incorrect code

### What's different from docs generate

Both write artifacts, but only `dbt docs generate` builds a documentation site you can view in a browser.

|                  | `--write-catalog` flag                                                                                         | `dbt docs generate` command                                                         |
| ---------------- | -------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| What it produces | `catalog.json`, plus the artifacts the command already generates                                               | `index.html`, `assets/`, and the index files                                        |
| What it's for    | Metadata for [Catalog](../../docs/explore/build-and-view-your-docs.md) and the metadata APIs | A static site you can preview with `dbt docs serve` or host anywhere                |
| Where it works   | Locally and in dbt platform. The command is added automatically in dbt platform jobs on dbt v2.                | Locally only in v2. dbt platform job runs use `dbt compile --write-catalog` instead |

## dbt platform behavior

dbt Docs v2 is built for local and self-hosted workflows. In dbt platform, use [Catalog](../../docs/explore/build-and-view-your-docs.md), which gives you a cloud-hosted, always-up-to-date view of your project:

* In dbt platform jobs running v2, every job run automatically refreshes Catalog metadata, so you don't need a separate docs step.
* If you add `dbt docs generate` as a job step, dbt automatically runs `dbt compile --write-catalog` instead and directs you to Catalog. The job doesn't produce the static site or `index.html`.
* To share your project with stakeholders who don't develop in dbt, add as many [read-only seats](../../docs/platform/manage-access/seats-and-users.md) as you need. Developer and read-only seats both include access to Catalog
