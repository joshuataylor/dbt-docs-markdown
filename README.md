# dbt-docs-markdown

Pre-compiled dbt documentation as plain markdown, rebuilt every two hours from the [dbt docs source](https://github.com/dbt-labs/docs.getdbt.com). Used by [Datamancer](https://datamancer.ai) to power dbt documentation in JetBrains IDEs (PyCharm, DataGrip, etc.).

## The problem

The dbt docs site is a Docusaurus app. Its `.md` URLs used to serve raw MDX source -- import statements and JSX components (`<FAQ>`, `<VersionBlock>`, `<File>`, etc.) passed through verbatim, partials (`/snippets/`) never inlined. That part is fixed upstream now ([#9765](https://github.com/dbt-labs/docs.getdbt.com/pull/9765)): the site generates compiled markdown from the rendered HTML. What remains is versioning: the site is built once at the default version, so its `.md` reflects a single version and everything gated behind `<VersionBlock>` for other versions is absent. This repo exists to produce a complete, correctly filtered set per supported dbt version.

## What this repo does

A GitHub Actions workflow runs every two hours and produces one set of `.md` files per supported dbt version under `docs/<product>/<version>/`:

```
docs/
  v2/
    2_0/    # the Fusion engine line (dbt platform)
  v1/
    1_12/   # dbt Core v1.12
    1_11/   # dbt Core v1.11
```

Each file is a fully rendered page: partials inlined, components expanded, with only the content appropriate for that version and product. The set of builds is derived dynamically from `dbt-versions.js` in the source repo, so new versions and products are picked up automatically.

## How it works

The markdown generator itself now lives upstream: [dbt-labs/docs.getdbt.com#9765](https://github.com/dbt-labs/docs.getdbt.com/pull/9765) replaced the raw-MDX-source generator with `@signalwire/docusaurus-plugin-llms-txt`, which derives per-page `.md` from the final rendered HTML -- partials inlined, components expanded, links rewritten to be document-relative. What upstream does not do is build per version, so this repo layers two things on top:

**1. Patch the source for static per-version output**

Before each build, the workflow applies the compare diff of
[`feature/fix-markdown`](https://github.com/joshuataylor/docs.getdbt.com/tree/feature/fix-markdown)
(the non-upstreamable delta, rebased onto upstream `current`; `git apply --allow-empty` tolerates the diff being empty if everything is ever merged). It currently carries:

- **VersionBlock SSR fix.** The `VersionBlock` component guards its content with a `loading` state that is always `true` during SSR and only cleared client-side, so every `<VersionBlock>` renders `null` in static HTML and version-gated content is silently absent. The patch removes the guard so SSR renders from the context version and product.
- **`SKIP_LINK_CHECK` support.** Per-version builds gate pages out on purpose, so links to them dangle; the patch downgrades `onBrokenLinks` from `throw` to `warn` only when the workflow sets `SKIP_LINK_CHECK=true`.
- **`data-md-hide` stripping.** A rehype plugin in the llms-txt conversion pipeline drops any element marked with a `data-md-hide` attribute from the generated markdown without touching the rendered site (first user: the "Was this page helpful?" feedback widget).

**2. Build once per (product, version)**

`VersionBlock` filters on both version and product name (`<VersionBlock firstVersion="2.0" product="Fusion">`). The workflow reads `dbt-versions.js` with Node.js to discover all unique (product, version) combinations, then for each: patches `VersionContext.js` to hard-code the appropriate subProduct as the default, runs a full Docusaurus build, and copies the resulting `.md` files to `docs/<name>/<version>/`.
