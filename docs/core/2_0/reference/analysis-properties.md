# Analysis properties

We recommend you define analysis properties in your `analyses/` directory, which is illustrated in the [`analysis-paths`](https://docs.getdbt.com/reference/project-configs/analysis-paths.md) configuration. You can name these files `whatever_you_want.yml`, and nest them arbitrarily deeply in subfolders within the `analyses/` or `models/` directory.

analyses/\<filename>.yml

```yml

analyses:
  - name: <analysis_name> # required
    description: <markdown_string>
    config:
      enabled: true | false
      docs: # changed to config in v1.10
        show: true | false
        node_color: <color_id> # Use name (such as node_color: purple) or hex code with quotes (such as node_color: "#cd7f32")
      tags: <string> | [<string>]
    columns:
      - name: <column_name>
        description: <markdown_string>
      - name: ... # declare properties of additional columns

  - name: ... # declare properties of additional analyses
```

To configure analyses at the project level, set the [`require_corrected_analysis_fqns`](https://docs.getdbt.com/reference/global-configs/behavior-flag-introduction.md#project-level-configuration-for-analyses) flag to `true` and add an `analyses` block in your `dbt_project.yml`. The project-level configuration applies to existing analyses in the `analyses/` folder — for example, setting `+enabled: false` disables them all.

Beta feature

The project-level configuration for analyses is a beta feature in dbt Core v1.12.

dbt\_project.yml

```yaml
flags:
  require_corrected_analysis_fqns: true

analyses:
  +enabled: true | false
```
