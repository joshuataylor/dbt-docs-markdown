# Snapshot properties

Define snapshot properties in YAML to document snapshots, configure settings, add tests, and describe columns.

In dbt v1.9 and later, snapshots are defined and configured in YAML files within your `snapshots/` directory (as defined by the [`snapshot-paths` config](https://docs.getdbt.com/reference/project-configs/snapshot-paths.md)). Snapshot properties are declared within these YAML files, allowing you to define both the snapshot configurations and properties in one place.

We recommend that you put them in the `snapshots/` directory. You can name these files `whatever_you_want.yml`, and nest them arbitrarily deeply in subfolders within the `snapshots/` or `models/` directory.

Learn by video!

For video tutorials on

<!-- -->

Snapshots

<!-- -->

, go to dbt Learn and check out the [Snapshots](https://learn.getdbt.com/courses/snapshots)

<!-- -->

[ course](https://learn.getdbt.com/courses/snapshots).

snapshots/\<filename>.yml

```yml

snapshots:
  - name: <snapshot name>
    description: <markdown_string>
    config:
      <snapshot_config>: <config_value>
      meta: {<dictionary>}
      docs:
        show: true | false
        node_color: <color_id> # Use name (such as node_color: purple) or hex code with quotes (such as node_color: "#cd7f32")
    data_tests:
      - <test>
      - ...
    columns:
      - name: <column name>
        description: <markdown_string>
        quote: true | false
        data_tests:
          - <test>
          - ... # declare additional tests
        config:
          meta: {<dictionary>}
          tags: [<string>]
      - ... # declare properties of additional columns

    - name: ... # declare properties of additional snapshots
```
