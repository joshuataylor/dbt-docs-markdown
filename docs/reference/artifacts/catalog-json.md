# Catalog JSON file

**Current schema**: [`v1`](https://schemas.getdbt.com/dbt/catalog/v1.json)

**Produced by:** [`docs generate`](https://docs.getdbt.com/reference/commands/cmd-docs.md)

This file contains information from your data warehouse about the tables and views produced and defined by the resources in your project. Today, dbt uses this file to populate metadata, such as column types and table statistics, in the [docs site](https://docs.getdbt.com/docs/explore/build-and-view-your-docs.md).

### Top-level keys[​](#top-level-keys "Direct link to Top-level keys")

* [`metadata`](https://docs.getdbt.com/reference/artifacts/dbt-artifacts.md#common-metadata)
* `nodes`: Dictionary containing information about database objects corresponding to dbt models, seeds, and snapshots.
* `sources`: Dictionary containing information about database objects corresponding to dbt sources.
* `errors`: Errors received while running metadata queries during `dbt docs generate`.

### Resource details[​](#resource-details "Direct link to Resource details")

Within `sources` and `nodes`, each dictionary key is a resource `unique_id`. Each nested resource contains:

* `unique_id`: `<resource_type>.<package>.<resource_name>`, same as dictionary key, maps to `nodes` and `sources` in the [manifest](https://docs.getdbt.com/reference/artifacts/manifest-json.md)

* `metadata`

  * `type`: table, view, etc.
  * `database`
  * `schema`
  * `name`
  * `comment`
  * `owner`

* `columns` (array)

  <!-- -->

  * `name`
  * `type`: data type
  * `comment`
  * `index`: ordinal

* `stats`: differs by database and relation type

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
