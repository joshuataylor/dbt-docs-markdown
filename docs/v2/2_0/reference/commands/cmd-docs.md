# About dbt docs commands

(Applies to dbt v2.0 and later)

With the dbt Fusion engine and dbt Core 2.0, [dbt Docs v2](../../docs/build/view-documentation.md#dbt-docs-v2) is the recommended way to generate and view your project's documentation. Use `dbt docs generate` to build the documentation site and `dbt docs serve` to preview it locally.

If you only need to hydrate catalog metadata (`catalog.json`) for Catalog in dbt platform, without building the documentation site, use the [`--write-catalog` flag](#--write-catalog-flag) instead.

## dbt Docs v2 [Beta](https://docs.getdbt.com/docs/dbt-versions/product-lifecycles "Go to https://docs.getdbt.com/docs/dbt-versions/product-lifecycles")

Instead of loading a static `manifest.json` in the browser, v2 produces Parquet artifacts when you compile or build your project. `dbt docs generate` exports a documentation site made of plain static files (a single-page app plus those artifacts) that any file host can serve. The browser reads the Parquet directly using DuckDB-WASM (WebAssembly), so you don't need to run a stateful server to view your docs. This keeps the experience fast even for large projects.

### Generate the site

`dbt docs generate` compiles your project, writes the index, and exports the documentation site in a single command:

```shell
dbt docs generate
```

By default, dbt writes the site into your `target/` directory (`target/index.html`, `target/assets/`, and the index under `target/index/`), matching the layout of dbt Core v1.x. You can serve `index.html` from `target/` the same way you did in v1, so an existing pipeline that runs `dbt docs generate && mv target public` keeps working.

Use `--output-dir` to write a self-contained copy of the site to a different directory:

```shell
dbt docs generate --output-dir site
```

This writes a `site/` directory (the app, hashed assets, and a copy of the index) that you can host on S3, GitHub Pages, Netlify, GitLab Pages, or any similar static file host.

To skip compilation and export whatever index is already on disk, use `--no-compile`, which fails with an error if no index exists:

```shell
dbt docs generate --no-compile
```

#### Column lineage and richer metadata

Column-level lineage and richer column metadata require an index built with [`--static-analysis strict`](../../docs/build/about-static-analysis.md). Because `dbt docs generate` runs a standard compile by default, build the index with strict static analysis first when you want column lineage, then export it:

```shell
dbt build --write-index --static-analysis strict
dbt docs generate --no-compile
```

If you generate the site without column lineage, dbt Docs v2 hides those features instead of showing empty data.

### Serve dbt Docs v2

To preview the site locally, run:

```shell
dbt docs serve
```

`dbt docs serve` generates the site if it's missing or older than the index, then serves the static files. The server starts on port `8580` by default and opens in your browser. Use `--port` to change the port:

```shell
dbt docs serve --port 8081
```

Use the `--target-path` flag to change the path where dbt reads artifacts from:

```shell
dbt docs serve --target-path ~/Developer/internal-analytics/target
```

Because the generated site is a set of static files, you can also host it on any static file host — such as cloud object storage or a static site host — instead of serving it locally.

### Project overview page

dbt Docs v2 renders your project's `__overview__` doc block as the landing page, the same as dbt Docs v1. dbt discovers overview content by scanning your `docs-paths` for `{% docs %}` blocks, so a block in `models/overview.md` is found by default. A file at `docs/overview.md` is only picked up when your project sets `docs-paths: ["docs"]`. If your project defines no overview, dbt renders its default overview content.

## --write-catalog flag

The `--write-catalog` flag generates the [`catalog.json`](../artifacts/catalog-json.md) artifact, which contains metadata about the tables and views produced by the models in your project. It focuses solely on metadata hydration and does not build the documentation site — use [dbt Docs v2](#dbt-docs-v2) for that.

For Fusion jobs running in dbt platform, dbt automatically runs `write-catalog` with `build` and `run` and hydrates your Catalog, so you don't need to include it manually. You can use this flag with the following commands:

* `dbt build`
* `dbt run`
* `dbt parse`
* `dbt compile`

**Example**:

```shell
dbt build --write-catalog
```

### Platform behavior

In dbt platform jobs running on Fusion, you don't need to change anything to hydrate catalog metadata. dbt runs `write-catalog` automatically with `build` and `run`, so you don't need to run a separate command. You can optionally include it when running `dbt parse` or `dbt compile`.

To produce the [dbt Docs v2](#dbt-docs-v2) static site in a job, run `dbt docs generate` as a job step or enable documentation generation in your job settings. Otherwise, the job hydrates catalog metadata but doesn't produce the static site.

### Local usage

When running Fusion locally, add the `--write-catalog` flag to your command to generate the catalog:

```shell
dbt build --write-catalog
```

### What's different from docs generate

The `--write-catalog` flag focuses solely on metadata hydration, generating the `catalog.json` file that powers [Catalog](../../docs/explore/build-and-view-your-docs.md) and metadata APIs. It does not generate the static documentation website files (`index.html`).
