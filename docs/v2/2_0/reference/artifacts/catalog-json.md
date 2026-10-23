# Catalog JSON file

**Current schema**: [`v1`](https://schemas.getdbt.com/dbt/catalog/v1.json)

**Produced by:** (Applies to dbt v2.0 and later) [`--write-catalog`](../commands/cmd-docs.md#--write-catalog-flag) flag

This file contains information from your data warehouse about the tables and views produced and defined by the resources in your project. (Applies to dbt v2.0 and later) [dbt Docs v2](../commands/cmd-docs.md#dbt-docs-v2) reads the [dbt Information Schema](../../docs/build/dbt-information-schema.md) rather than this file. `catalog.json` remains available as a separate artifact for [Catalog](../../docs/explore/explore-projects.md) and external tools.

### Top-level keys

* [`metadata`](./dbt-artifacts.md#common-metadata)
* `nodes`: Dictionary containing information about database objects corresponding to dbt models, seeds, and snapshots.
* `sources`: Dictionary containing information about database objects corresponding to dbt sources.
* `errors`: Errors received while running metadata queries during (Applies to dbt v2.0 and later) catalog generation (via `--write-catalog` flag).

### Resource details

Within `sources` and `nodes`, each dictionary key is a resource `unique_id`. Each nested resource contains:

* `unique_id`: `<resource_type>.<package>.<resource_name>`, same as dictionary key, maps to `nodes` and `sources` in the [manifest](./manifest-json.md)

* `metadata`

  * `type`: table, view, etc.
  * `database`
  * `schema`
  * `name`
  * `comment`
  * `owner`

* `columns` (array)

  * `name`
  * `type`: data type
  * `comment`
  * `index`: ordinal

* `stats`: differs by database and relation type
