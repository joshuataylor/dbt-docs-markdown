# dbt-docs-markdown

Pre-compiled dbt documentation as plain markdown, rebuilt every two hours from the [dbt docs source](https://github.com/dbt-labs/docs.getdbt.com).

[Datamancer](https://github.com/joshuataylor/datamancer) uses it to serve dbt documentation inside JetBrains IDEs (PyCharm, DataGrip).

Hopefully you'll find it useful in your ripgrepping!

## Why this exists

The dbt docs site is a Docusaurus app, and trying to use something like ripgrep is annoying.

The [docs.getdbt.com](https://docs.getdbt.com) website DOES support `.md` as a file extension (e.g. https://docs.getdbt.com/docs/fusion/about-fusion.md), but I wanted a way to get the final, compiled result as plaintext (markdown).

### Upstreaming Patches

Most changes can be upstreamed - a few patches here remain as either experimental, but the preferred approach is to upstream every change/fix back to the [docs.getdbt.com](https://github.com/dbt-labs/docs.getdbt.com) repository, especially as the docs folks over at dbt Labs have been a pleasure to work with, and receptive to proposed changes.

Until August 2026, the compiled dbt documentation needed a bunch of fixes (which I maintained in a branch), which are now upstreamed! :tada.

See [Pull Requests in docs.getdbt.com](https://github.com/dbt-labs/docs.getdbt.com/issues?q=is%3Apr%20author%3A%40me%20sort%3Aupdated-desc):

- [Fix whitespace inside code fences and inline code - #9832](https://github.com/dbt-labs/docs.getdbt.com/pull/9832)
- [Strip React SSR comment markers from generated llms-txt markdown - #9772](https://github.com/dbt-labs/docs.getdbt.com/pull/9772)
- [Remove duplicated entries in dbt-versions - #9783](https://github.com/dbt-labs/docs.getdbt.com/pull/9783)
- [Fix broken tables in generated markdown (Loading table/Search table..) - #9769](https://github.com/dbt-labs/docs.getdbt.com/pull/9769)
- [Give each tab panel a labelled heading in generated markdown - #9773](https://github.com/dbt-labs/docs.getdbt.com/pull/9773)
- [Fix mistyped Constant, Term, and Lifecycle names in docs - #9776](https://github.com/dbt-labs/docs.getdbt.com/pull/9776)
- [Close the unterminated code fence in the alias snapshot example - #9771](https://github.com/dbt-labs/docs.getdbt.com/pull/9771)
- [Generate compiled per-page markdown from rendered HTML - #9765](https://github.com/dbt-labs/docs.getdbt.com/pull/9765)
- [Only show supported dbt Core features in the sidebar - #9745](https://github.com/dbt-labs/docs.getdbt.com/pull/9745)
- [Fix markdown for datamancer - #9470](https://github.com/dbt-labs/docs.getdbt.com/pull/9470)
- [Create rehype plugin that readds markdown codeblock language to codeblocks - #8684](https://github.com/dbt-labs/docs.getdbt.com/pull/8684)

## GitHub Action

A GitHub Actions workflow runs every two hours and writes one set of `.md` files per supported dbt version under `docs/<product>/<version>/`:

```
docs/
  v2/
    2_0/    # the Fusion engine line (dbt platform)
  v1/
    1_12/   # dbt Core v1.12
    1_11/   # dbt Core v1.11
```

Every file is a fully rendered page -- partials inlined, components expanded, carrying only the content that applies to that version and product.

The list of builds comes from `dbt-versions.js` in the source repo, so new versions and products are picked up on their own.

### Patches

- Remove `Was this page helpful?` widget at the end of every page.
- Strip DocCard icon glyph from generated card-list Markdown, for example: https://docs.getdbt.com/category/project-configs.md
  Before:
    ~~~
    ## [📄️ dbt\_project.yml](../reference/dbt_project.yml.md)
    
    [Reference guide for configuring the dbt\_project.yml file.](../reference/dbt_project.yml.md)
    ~~~

  After:
    ~~~
    ## [dbt\_project.yml](../reference/dbt_project.yml.md)
    
    [Reference guide for configuring the dbt\_project.yml file.](../reference/dbt_project.yml.md)
    ~~~
- Convert <br> to line breaks in generated Markdown, for example: https://docs.getdbt.com/reference/function-configs.md
  Before:
    ~~~
    ### Function-specific configurations
    
    Resource-specific configurations are applicable to only one dbt resource type rather than multiple resource types. You can define these settings in the project file (`dbt_project.yml`), a property file (`models/properties.yml` for models, similarly for other resources), or within the resource’s file using the `{{ config() }}` macro.<br />
    ~~~

  After:
    ~~~
    ### Function-specific configurations
    
    Resource-specific configurations are applicable to only one dbt resource type rather than multiple resource types. You can define these settings in the project file (`dbt_project.yml`), a property file (`models/properties.yml` for models, similarly for other resources), or within the resource’s file using the `{{ config() }}` macro.
    ~~~
