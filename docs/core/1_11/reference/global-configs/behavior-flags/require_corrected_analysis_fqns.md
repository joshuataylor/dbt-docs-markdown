# Project-level configuration for analyses

Beta feature

The project-level configuration for analyses is a beta feature in dbt Core v1.12.

| require\_corrected\_analysis\_fqns | dbt **Latest** | dbt Core |
| ---------------------------------- | -------------- | -------- |
| Introduced                         | 2026.3         | 1.12.0   |
| Matured (default → `true`)         | —              | —        |
| Removed                            | —              | —        |

Search table...

|                  |   |   |   |   |
| ---------------- | - | - | - | - |
| Loading table... |   |   |   |   |

Previously, project-level configuration for [analyses](https://docs.getdbt.com/docs/build/analyses.md) in `dbt_project.yml` was silently ignored. Fully qualified names (FQNs) for analyses also contained an extra `analyses` path segment that was inconsistent with other resource types.

When `require_corrected_analysis_fqns` is set to `true`, dbt:

* Routes analysis configurations from the `analyses` block in `dbt_project.yml`, enabling project-level configurations to take effect.
* Removes the extra FQN segment so that analysis FQNs are consistent with other resource types (for example, `your_project.my_analysis` instead of `your_project.analyses.my_analysis`).

To configure analyses at the project level, set the [`require_corrected_analysis_fqns`](https://docs.getdbt.com/reference/global-configs/behavior-flags/require_corrected_analysis_fqns.md) flag to `true` and add an `analyses` block in your `dbt_project.yml`. The project-level configuration applies to existing analyses in the `analyses/` folder — for example, setting `+enabled: false` disables them all.

dbt\_project.yml

```yaml
flags:
  require_corrected_analysis_fqns: true

analyses:
  +enabled: true | false
```

For more information, refer to [Analyses](https://docs.getdbt.com/docs/build/analyses.md) and [Analysis properties](https://docs.getdbt.com/reference/analysis-properties.md).
