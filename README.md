# dbt-docs-markdown

Pre-compiled dbt documentation as plain markdown, rebuilt every two hours from the [dbt docs source](https://github.com/dbt-labs/docs.getdbt.com). Used by [Datamancer](https://datamancer.ai) to power dbt documentation in JetBrains IDEs (PyCharm, DataGrip, etc.).

## The problem

The dbt docs site is a Docusaurus app. It does expose `.md` URLs, but those serve raw MDX source -- import statements and JSX components (`<FAQ>`, `<VersionBlock>`, `<File>`, etc.) are passed through verbatim rather than rendered. Content in partials (`/snippets/`) is never inlined. Version-gated content inside `<VersionBlock>` is not expanded. The result is markdown that is largely unparseable by anything that isn't the Docusaurus runtime.

## What this repo does

A GitHub Actions workflow runs every two hours and produces one set of `.md` files per supported dbt version under `docs/<version>/`:

```
docs/
  fusion/
    2_0/    # version=2.0&name=Fusion (dbt platform stable)
  core/
    2_0/    # version=2.0&name=Core (dbt Core v2.0 alpha)
    1_12/   # version=1.12&name=Core (dbt platform latest)
    1_11/   # version=1.11&name=Core
```

Each file is a fully rendered page: partials inlined, components expanded, with only the content appropriate for that version and product. The set of builds is derived dynamically from `dbt-versions.js` in the source repo, so new versions and products are picked up automatically.

## How it works

The workflow patches the upstream Docusaurus source before each build to fix three things that prevent clean static output:

**1. Switch the markdown generator**

The dbt docs repo ships two markdown generators. The default one (`buildRawMarkdownData`) emits raw MDX source. The other (`@signalwire/docusaurus-plugin-llms-txt`) generates markdown from the final rendered HTML -- with partials inlined and components expanded -- but is disabled to avoid a route conflict with the first. The workflow disables `buildRawMarkdownData` and enables the llms-txt generator instead.

**2. Fix VersionBlock for static builds**

The `VersionBlock` React component guards its content with a `loading` state that is always `true` during SSR and only cleared client-side. This means every `<VersionBlock>` returns `null` in the static HTML regardless of which version or product is active -- all version-gated content is silently absent from a normal build.

The workflow removes the loading guard so SSR renders based on the context version and product. Without this, switching the default version would have no effect on the output.

**3. Build once per (product, version)**

`VersionBlock` filters on both version and product name (`<VersionBlock firstVersion="2.0" product="Fusion">`). The workflow reads `dbt-versions.js` with Node.js to discover all unique (product, version) combinations, then for each: patches `VersionContext.js` to hard-code the appropriate subProduct as the default, runs a full Docusaurus build, and copies the resulting `.md` files to `docs/<name>/<version>/`.
