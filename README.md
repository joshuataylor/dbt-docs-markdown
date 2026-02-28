# dbt-docs-markdown

The [dbt docs](https://github.com/dbt-labs/docs.getdbt.com) site uses Docusaurus, which strips language identifiers from code blocks during the build. A fenced block written as ` ```sql ` in the source ends up as a bare ` ``` ` in the compiled `.md` files (e.g. [source.md](https://docs.getdbt.com/reference/dbt-jinja-functions/source.md)).

This repository adds them back in via a [rehype plugin](https://docusaurus.io/docs/markdown-features/plugins).

Built for a JetBrains IDE plugin that shows dbt Jinja documentation on hover — without the language tags, code blocks lose syntax highlighting.

Related upstream PR: [dbt-labs/docs.getdbt.com#8684](https://github.com/dbt-labs/docs.getdbt.com/pull/8684)
