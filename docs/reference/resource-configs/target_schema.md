# target\_schema

note

Starting in dbt Core v1.9+, this functionality is no longer utilized. Use the [schema](https://docs.getdbt.com/reference/resource-configs/schema.md) config as an alternative to define a custom schema while still respecting the `generate_schema_name` macro.

Try it now in the [dbt **Latest** release track](https://docs.getdbt.com/docs/dbt-versions/cloud-release-tracks.md).

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

## Description[​](#description "Direct link to Description")

The schema that dbt should build a [snapshot](https://docs.getdbt.com/docs/build/snapshots.md) table into. When `target_schema` is provided, snapshots build into the same `target_schema`, no matter who is running them.

On **BigQuery**, this is analogous to a `dataset`.

## Default[​](#default "Direct link to Default")

<!-- -->

## Examples[​](#examples "Direct link to Examples")

### Build all snapshots in a schema named `snapshots`[​](#build-all-snapshots-in-a-schema-named-snapshots "Direct link to build-all-snapshots-in-a-schema-named-snapshots")

dbt\_project.yml

```yml
snapshots:
  +target_schema: snapshots
```

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
