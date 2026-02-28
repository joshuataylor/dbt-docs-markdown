# invalidate\_hard\_deletes legacy

Legacy opt-in configuration to enable invalidating hard deleted records while snapshotting the query.

This is a legacy config — Use the [`hard_deletes`](https://docs.getdbt.com/reference/resource-configs/hard-deletes.md) config instead.

In dbt release tracks and dbt Core 1.9 and higher, the [`hard_deletes`](https://docs.getdbt.com/reference/resource-configs/hard-deletes.md) config replaces the `invalidate_hard_deletes` config for better control over how to handle deleted rows from the source.

For new snapshots, set the config to `hard_deletes='invalidate'` instead of `invalidate_hard_deletes=true`. For existing snapshots, [arrange an update](https://docs.getdbt.com/reference/snapshot-configs.md#snapshot-configuration-migration) of pre-existing tables before enabling this setting.

<!-- -->

dbt\_project.yml

```yml
snapshots:
  <resource-path>:
    +strategy: timestamp
    +invalidate_hard_deletes: true
```

## Description[​](#description "Direct link to Description")

Opt-in feature to enable invalidating hard deleted records while snapshotting the query.

## Default[​](#default "Direct link to Default")

By default the feature is disabled.

## Example[​](#example "Direct link to Example")

## Was this page helpful?

YesNo

[Privacy policy](https://www.getdbt.com/cloud/privacy-policy)[Create a GitHub issue](https://github.com/dbt-labs/docs.getdbt.com/issues)

This site is protected by reCAPTCHA and the Google [Privacy Policy](https://policies.google.com/privacy) and [Terms of Service](https://policies.google.com/terms) apply.
