# target\_schema

note

Starting in dbt Core v1.9+, this functionality is no longer utilized. Use the [schema](./schema.md) config as an alternative to define a custom schema while still respecting the `generate_schema_name` macro.

Try it now in the [dbt **Latest** release track](../../docs/dbt-versions/dbt-release-tracks.md).

dbt\_project.yml

```yml
snapshots:
  <resource-path>:
    +target_schema: string
```

snapshots/\<filename>.sql

```jinja2
{{ config(
      target_schema="string"
) }}
```

## Description

The schema that dbt should build a [snapshot](../../docs/build/snapshots.md) table into. When `target_schema` is provided, snapshots build into the same `target_schema`, no matter who is running them.

On **BigQuery**, this is analogous to a `dataset`.

## Default

(Applies to dbt v1.9 and later) In dbt Core v1.9+ and dbt **Latest** release track, this is not a required parameter.

## Examples

### Build all snapshots in a schema named `snapshots`

dbt\_project.yml

```yml
snapshots:
  +target_schema: snapshots
```
