# MetricFlow time spine YAML

Removed in dbt Core v2

This flag was removed in dbt Core v2 and in Fusion. The new behavior is always enabled. If you're upgrading, remove this flag from your `dbt_project.yml`.

| require\_yaml\_configuration\_for\_mf\_time\_spines | dbt **Latest** | dbt Core |
| --------------------------------------------------- | -------------- | -------- |
| Introduced                                          | 2024.10        | 1.9.0    |
| Matured (default → `true`)                          | Sep 1, 2026    | 1.12.0   |
| Removed                                             | —              | v2.0     |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

<br />

In previous versions (dbt Core 1.8 and earlier), the MetricFlow time spine configuration was stored in a `metricflow_time_spine.sql` file.

Starting in dbt Core v1.12, this flag defaults to `true`. When enabled, dbt suppresses the `MFTimespineWithoutYamlConfigurationDeprecation` deprecation warning. The legacy SQL file configuration continues to work in both cases.

The MetricFlow properties YAML file should have the `time_spine:` field. Refer to [MetricFlow timespine](https://docs.getdbt.com/docs/build/metricflow-time-spine.md) for more details.

## Impact[​](#impact "Direct link to Impact")

This flag has no functional impact; the legacy time-spine model continues to work in both cases. The only visible changes are:

* The `MFTimespineWithoutYamlConfigurationDeprecation` warning no longer appears in logs.
* If you use `--warn-error`, the warning no longer fires and will no longer escalate to an error.

 Opting out of this flag

No action is required for most projects. The legacy `metricflow_time_spine.sql` model continues to work with or without this flag.

If you rely on the `MFTimespineWithoutYamlConfigurationDeprecation` warning firing under `--warn-error` to enforce a YAML migration, you can opt out by setting the flag to `false` in `dbt_project.yml`:

dbt\_project.yml

```yaml
flags:
  require_yaml_configuration_for_mf_time_spines: false
```

To remove the deprecation warning permanently, migrate `metricflow_time_spine.sql` to a YAML `time_spine` block under a model entry in `models:`. Refer to [MetricFlow timespine](https://docs.getdbt.com/docs/build/metricflow-time-spine.md) for the current syntax.
