# invalidate\_hard\_deletes legacy

Legacy opt-in configuration to enable invalidating hard deleted records while snapshotting the query.

This is a legacy config — Use the [`hard_deletes`](https://docs.getdbt.com/reference/resource-configs/hard-deletes.md) config instead.

In dbt release tracks and dbt Core 1.9 and higher, the [`hard_deletes`](https://docs.getdbt.com/reference/resource-configs/hard-deletes.md) config replaces the `invalidate_hard_deletes` config for better control over how to handle deleted rows from the source.

For new snapshots, set the config to `hard_deletes='invalidate'` instead of `invalidate_hard_deletes=true`. For existing snapshots, [arrange an update](https://docs.getdbt.com/reference/snapshot-configs.md#snapshot-configuration-migration) of pre-existing tables before enabling this setting.

snapshots/\<filename>.yml

```yaml
snapshots:
  - name: snapshot
    relation: source('my_source', 'my_table')
    config:
      strategy: timestamp
      invalidate_hard_deletes: true | false
```

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

snapshots/orders.yml

```yaml
snapshots:
  - name: orders_snapshot
    relation: source('jaffle_shop', 'orders')
    config:
      schema: snapshots
      database: analytics
      unique_key: id
      strategy: timestamp
      updated_at: updated_at
      invalidate_hard_deletes: true
```
