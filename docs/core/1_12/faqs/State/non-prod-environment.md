# What if my prod environment isn't named prod?

You can specify the defer-to environment using the [`defer_to_target`](https://docs.getdbt.com/reference/resource-configs/defer-to-target.md) config in `profiles.yml`:

```yaml
my_project:
  outputs:
    prod:
      type: snowflake
      defer_to_target: production
```

`defer_to_target` only applies to self-managed deployments. If you're using the dbt platform, deferral is configured through your environment settings in the UI. For more details, refer to [Configuring deferral](https://docs.getdbt.com/docs/deploy/dbt-state-deferral.md).
