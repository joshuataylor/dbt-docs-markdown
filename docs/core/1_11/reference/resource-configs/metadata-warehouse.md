# metadata\_warehouse

Snowflake only

This configuration is available in Snowflake only.

profiles.yml

```yaml
my_project:
  outputs:
    prod:
      type: snowflake
      # ... other connection settings
      warehouse: TRANSFORMING
      metadata_warehouse: METADATA_XS
  target: prod
```

## Definition[​](#definition "Direct link to Definition")

dbt State performs metadata introspection queries to determine whether models need to be rebuilt. On Snowflake, these queries run against your configured `warehouse` by default, which can cause queuing when your primary warehouse is under heavy load.

`metadata_warehouse` lets you route dbt State metadata lookups to a separate, smaller warehouse — keeping introspection overhead off your main compute resource.

If omitted, dbt State falls back to the `warehouse` setting in `profiles.yml`.

You can also use `metadata_warehouse` on the dbt platform by adding it as an [extended attribute](https://docs.getdbt.com/docs/dbt-platform-environments.md#extended-attributes) in your environment settings.

note

This configuration currently applies only to dbt State metadata queries.

## Default[​](#default "Direct link to Default")

Falls back to the `warehouse` setting in `profiles.yml`.

## Example[​](#example "Direct link to Example")

### Separate metadata and execution warehouses[​](#separate-metadata-and-execution-warehouses "Direct link to Separate metadata and execution warehouses")

profiles.yml

```yaml
my_project:
  outputs:
    prod:
      type: snowflake
      account: abc12345
      database: ANALYTICS
      schema: DBT_PROD
      warehouse: TRANSFORMING          # used for model execution
      metadata_warehouse: METADATA_XS  # used for dbt State introspection
  target: prod
```

## Related docs[​](#related-docs "Direct link to Related docs")

* [About dbt State](https://docs.getdbt.com/docs/deploy/dbt-state-about.md)
* [Snowflake configuration](https://docs.getdbt.com/reference/resource-configs/snowflake-configs.md)
